# OtherSummary



## 一、HashMap

- `map.get(key)` 不能使用 `++`运算符，可以使用 `+1` 代替；

- 遍历 Key 和 Value 的方式：

    ```java
    HashMap<Integer, Integer>() map  = new HashMap<>();  
    for (Integer key : map.keySet()){
    }
    for(Integer value : map.values()){
    }
    ```

- 已经含有 Key 则对应 value + 1，否则放入

    ```java
    HashMap<Integer, Integer> map = new HashMap<>();
    // 这里的 defaultValue 设置为 0，根据题意设置即可
    map.put(key, map.getOrDefault(key, 0) + 1);
    ```

    



## 二、常见特殊情况

- 只有一个元素
- 为空
- 只有一个元素且该元素为 0；



## 三、求中点

首先假设我们的变量都是 int 值。

二分查找中我们需要根据 start 和 end 求中点，正常情况下加起来除以 2 即可。

int mid = (start + end) / 2
但这样有一个缺点，我们知道int的最大值是 Integer.MAX_VALUE ，也就是2147483647。那么有一个问题，如果 start = 2147483645，end = = 2147483645，虽然 start 和 end都没有超出最大值，但是如果利用上边的公式，加起来的话就会造成溢出，从而导致mid计算错误。

解决的一个方案就是利用数学上的技巧，我们可以加一个 start 再减一个 start 将公式变形。

`(start + end) / 2 = (start + end + start - start) / 2 = start + (end - start) / 2`
这样的话，就解决了上边的问题。

然后当时和同学看到jdk源码中，求mid的方法如下

`int mid = (start + end) >>> 1`
它通过移位实现了除以 2，但。。。这样难道不会导致溢出吗？

首先大家可以补一下 补码 的知识。

其实问题的关键就是这里了`>>>` ，我们知道还有一种右移是 `>>`。区别在于 `>>` 为有符号右移，右移以后最高位保持原来的最高位。而 `>>>` 这个右移的话最高位补 0。

所以这里其实利用到了整数的补码形式，最高位其实是符号位，所以当 start + end 溢出的时候，其实本质上只是符号位收到了进位，而 `>>>` 这个右移不仅可以把符号位右移，同时最高位只是补零，不会对数字的大小造成影响。

但>>有符号右移就会出现问题了，事实上 JDK6 之前都用的>>，这个 BUG 在 java 里竟然隐藏了十年之久。

**总**
经过这么多的分析，大家估计体会到了递归的魅力了吧，简洁而优雅。另外的两种迭代的实现，可以让我们更清楚的了解递归到底发生了什么。关于求中点，大家以后就用>>>吧，比start + (end - start) / 2简洁不少，还能给别人科普一下补码的知识。

