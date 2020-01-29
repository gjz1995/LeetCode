# ArraySummary

- 如果需要两次遍历数组查找元素是否在数组中，可以使用哈希表将数组中元素存放起来，这样第二次判断时候时间复杂度就是 O（1），即采用空间换取时间。-》Array-Easy-01

- 产生一个随机整数（包括正负）：`int randomValue = new Random().nextInt()`，这里的`Random()` 是 `java.util` 包中的，使用 `Math.Random()` 不能实现；-》Array-Easy-07

- 产生一个随机正整数（限定上限值）：`array[i] = (int) (Math.random() * 100);`生成 0 ~ 99 之间的整数值，如果要求有序，可以在生成之后使用：`Arrays.sort(array,begin,end)`对 array 数组在 begin 到 end 范围内进行排序。

- 获取一个数共几位：

    ```java
    int div = 1;
    // 相当于获取 X 一共几位数，例如 x = 7324,则 div = 1000,在下面相除得到首位 7
    while (x / div >= 10) {
        div *= 10;
    }
    ```

- 字符串 s 的长度：`s.length()` ，字符串可以使用 for 循环进行遍历，每个字符值为：`char a = s.charAt(i)`，各个字符连接成字符串：`”" + s.charAt(i) + s.charAt(i + 1)`;

- 当循环遍历过程中出现 `i + 1` 的时候，循环结束条件为`i < s.length() - 1`，同时如果每次处理的是当前 i 位置的元素，==不要忘记最后一个位置元素值==。

- 数字、字符串、字符数组转换

    ```java
    // 数字转换为字符串
    int   a = 332;
    String str = String.valueOf(a);
    //字符串转换为数字
    String str = "abcd";
    int a = Integer.parseInt(str);
    //字符串转化为字符数组
    String str = " acbd";
    char[] c = str.toCharArray();
    //字符数组转化为字符串
    char[] c = {'a','b','c'};
    String str = String.valueOf(c);
    
    ```

    整数转换为数组

    ```java
    public class Test{
        public static void main(String[]args){
            int a = 2017;
            int[] arr = transfer(a);
            for(int i=0; i<arr.length; i++){
                System.out.println(arr[i]);
            }
        }
        public static int[] transfer(int a ){
            String str =null;
            str = Integer.toString(a);
            int[] arr = new int[str.length()];
            for(int i=0; i<arr.length; i++){
                char c = str.charAt(i);
                String s = String.valueOf(c);
                int num = Integer.parseInt(s);
                arr[i] = num;
            }
        return arr;
        }
    }
    ```

- 矩阵行列重构（或者使用队列 566）

    ```java
    class Solution {
        public int[][] matrixReshape(int[][] nums, int r, int c) {
            // 合法性判断省略
    
            int total = r * c;
            int[][] res = new int[r][c];
            for(int t = 0; t < total; t++) {
                res[t/c][t%c] = nums[t/column][t%column];
            }
    
            return res;
        }
    }
    
    ```

    