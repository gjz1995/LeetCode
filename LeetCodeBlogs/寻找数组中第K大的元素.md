# 寻找数组中第K大的元素


这算是一道相当经典的算法题了：

在长度为N的乱序数组中寻找第k（n>=k）大的元素。

扩展思考：如何处理数组中的重复元素？比如，对于数组a={1,2,2,2,3,3,3}，第二大的元素应该是3还是2呢？

本文作这种分类：

如果第二大的元素是3，说明在处理第k大的元素时不处理重复的数据，也就是将原数组进行降序排序后，下标为k-1的元素。这种处理方法称之为“不处理重复数据方法”；

如果第二大的元素是2，说明已经忽略重复的数据了，这时候需要首先对a进行去重处理（可以用哈希表），然后再进行讨论。这种处理方法称之为“去除重复数据方法”。

下面的方法都默认为第一种。

**（1）最简单直接的方法：先排序再找**

最简单直接的想法是首先进行排序。假设元素的数量不大，比如才几千个，那就可以先进行排序，比如用快排或堆排，平均时间复杂度为O(N*logN)，然后取出前k个，于是总时间复杂度为O(NlogN)+O(k)=O(NlogN)。当然这种做法是浪费了不少的时间的，因为题目只要求找出第k大的元素，而不需要数据是有序的。

**（2）比较直接的方法：部分元素排序**

当k比较小的时候，k趟排序是个比较不错的方法。我们只需要排序最大的k个元素即可，剩下那些元素不需要管。最简单明了的就是在冒泡排序中只进行k趟起泡：

```
#include<iostream>
using namespace std;

int findMaxK(int a[], int n, int k) {

    //进行k趟起泡即可
    if (k == n) k = n - 1;
    bool flag;
    for (int i = 0; i < k; i++) {
        flag = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (a[j] > a[j + 1]) {
                int tmp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = tmp;
                if (!flag) flag = true;
            }
        }
        if (!flag) break;
    }
    return a[n - k];
}



int main() {
    int A[] = { 5,1,2,2,3,4,3,10};
    int n=8,k =5;
    int result= findMaxK(A, n, k);
    cout << "第" << k << "大的数字为" << result << endl;
    system("pause");

    return 0;

}
```

用上述方法，时间复杂度为O(N*k)，适用于k相对于N很小的情况。

**（3）快排的分治法**

快速排序使用了分治法的策略。它的基本思想是，选择一个基准数（一般称之为枢纽元），通过一趟排序将要排序的数据分割成独立的两部分：在枢纽元左边的所有元素都不比它大，右边所有元素都比它大，此时枢纽元就处在它应该在的正确位置上了。

在本问题中，假设有N个数存储在数组a中。我们从a中随机找出一个元素作为枢纽元，把数组分为两部分。其中左边元素都不比枢纽元大，右边元素都不比枢纽元小。此时枢纽元所在的位置记为mid。

如果右半边（包括a[mid]）的长度恰好为k，说明a[mid]就是需要的第k大元素，直接返回a[mid]。

如果右半边（包括a[mid]）的长度大于k，说明要寻找的第k大元素就在右半边，往右半边寻找。

如果右半边（包括a[mid]）的长度小于k，说明要寻找的第k大元素就在左半边，往左半边寻找。

```
#include<iostream>
#include<ctime>
using namespace std;

int divide(int a[], int low,int high) {
	//随机选一个元素作为枢纽元素
	//左边都是比枢纽元素小的，右边都是比枢纽元素大的
	srand((unsigned)time(NULL));
	int idx = (rand() % (high - low + 1)) + low;
	int tmp = a[low];
	a[low] = a[idx];
	a[idx] = tmp;
	tmp = a[low];

	while (low < high) {
		while (low<high && a[high] >= tmp) high--;
		if (low < high) {
			a[low] = a[high];
			low++;
		}

		while (low < high && a[low] <= tmp) low++;
		if (low < high) {
			a[high] = a[low];
			high--;
		}
	}

	//此时low=high，且low就是枢纽元应该在的位置编号，返回low
	a[low] = tmp;
	return low;
}



int findKMax(int a[],int low,int high,int k) {
	int mid = divide(a, low, high);
	//包括a[mid]的右半边长度
	int length_of_right = high - mid + 1;
	if (length_of_right == k) return a[mid];
	else if (length_of_right > k) {
		//右半边长度比k长，说明第k大的元素还在右半边，因此在右半边找
		return findKMax(a, mid + 1, high, k);
	}
	else {
		return findKMax(a, low, mid - 1, k - length_of_right);
	}
}





int main() {
	int A[] = { 1,2,2,2,3,3,3 };
	int n=7,k = 3;
	int result= findMaxK(A, 0,n-1, k);
	cout << "第" << k << "大的数字为" << result << endl;
	system("pause");
	return 0;

}
```

时间复杂度为O(N*logk)。当然，如果每次选择的枢纽元素都是最坏的那个，时间复杂度就会退化为O(N*k)。

（4）借助优先级队列

前面的解法都可以认为是排序算法的变种，需要对原数组进行多次访问。如果原数组特别大（上百万，甚至上亿），以至于无法存放在内存中，前面的方法就不适用了（毕竟访问外存储器的代价太大）。如果k足够小（k个数据足以放入内存），可以借助二叉堆来完成。这种解法在剑指offer中有所提及，先建立一个规模为k的最小化堆，然后每次拿待处理的数组中元素和堆的最小元素（根结点元素值）比较。如果待插入元素大于根结点元素，则在堆中删除根结点，并把待处理的元素入堆。否则可以抛弃这个元素。

下面用STL标准库中的priority_queue来模拟这个过程：

```
#include<iostream>

#include<queue>

using namespace std;

struct cmp
{
	bool operator()(int &a, int &b) const
	{
	  //因为优先出列判定为!cmp，所以反向定义实现最小值优先
		return a > b;
	}
};



int findMaxK(int a[], int n, int k) {
	priority_queue<int,vector<int>,cmp> myqueue;
	for (int i = 0; i < n; i++) {
		if (myqueue.size() < k) {
			myqueue.push(a[i]);
		}
		else {
			//将最小元素与a[i]比较
			int min = myqueue.top();
			if (a[i] > min) {
				myqueue.pop();
			}
			myqueue.push(a[i]);
		}
	}
	return myqueue.top();

}



int main() {
	int A[] = { 1,2,2,2,3,3,3 };
	int n=7,k = 7;
	int result= findMaxK(A, n, k);
	cout << "第" << k << "大的数字为" << result << endl;
	system("pause");
	return 0;

}
```

时间复杂度为O(N*logk)，因为二叉堆的插入和删除操作都是logk的时间复杂度。上述代码是“不处理重复数据方法”。

如果要求去除重复数据，那用平衡二叉搜索树会更合适。STL标准库中提供的红黑树实现的set就可以解决这个问题。

（5）键值索引法

这个方法对原数组a有要求：所有 N 个数都是正整数，且它们的取值范围不太大（假设a的所有元素都位于区间[0,max]）。此时可以申请一个规模为max+1的数组count，将count的全部元素初始化为0。然后利用count记录a中每个元素出现的个数。这样就可以在O(N)时间复杂度下找到a的第k大元素。

```
#include<iostream>
#include<vector>
using namespace std;

struct cmp
{
	bool operator()(int &a, int &b) const
	{
		//因为优先出列判定为!cmp，所以反向定义实现最小值优先
		return a > b;
	}

};



int findMaxK(int a[], int n, int k,int max) {
	vector<int> count(max + 1);
	for (int i = 0; i < max; i++) {
		count[i] = 0;
	}

	for (int j = 0; j < n; j++) {
		count[a[j]]++;
	}
	int ind = max;
	while (k > 0) {
    	if (count[ind] == 0) ind--;
		else {
			count[ind]--;
			k--;
		}
	}
	return ind;

}



int main() {

	int A[] = { 5,1,2,2,3,4,3,10};
	int n=8,k = 6;
	int result= findMaxK(A, n, k, 10);
	cout << "第" << k << "大的数字为" << result << endl;
	system("pause");
	return 0;

}
```

你可能会觉得，这种方法实在是好，只需O(N)的时间复杂度。但仔细想想，上面代码实现的键值索引法并不一定是O(N)的时间复杂度。确实，一开始构建count数组时，只需访问原数组a一次即可，时间为O(N)，但count构建结束后，从count中找到第k大的元素却并不是O(N）时间。大家不妨考虑一种情况：如果max比N大得多呢？比如要找a={1,2,3,100,100}的第3大元素，首先构建一个长度为101的数组，得把数组的全部元素置为0，O(max)。然后用count统计a中每个整数出现的次数，O(N)。最后寻找第3大元素，cout的访问次数为接近100次。因此，这个方法实际的时间复杂度为O(max)。

结论：这种方法的时间复杂度为O(max)，当max与N接近时，才能获得较好的性能。若max远大于N，就是个不好的方法。

