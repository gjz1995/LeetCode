# ArraySummary

- 如果需要两次遍历数组查找元素是否在数组中，可以使用哈希表将数组中元素存放起来，这样第二次判断时候时间复杂度就是 O（1），即采用空间换取时间。-》Array-Easy-01

- 产生一个随机整数（包括正负）：`int randomValue = new Random().nextInt()`，这里的`Random()` 是 `java.util` 包中的，使用 `Math.Random()` 不能实现；-》Array-Easy-07

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