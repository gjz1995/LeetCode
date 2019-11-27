# NumberFormatException相关的error总结

今天出现了个数字转换异常，处理好后稍微总结了几个出现情景。
致命异常: `java.lang.NumberFormatException: Invalid int: "0 "`

## 一、含义
java.lang.NumberFormatException　数字格式异常。
**当试图将一个 String 转换为指定的数字类型，而该字符串确不满足数字类型要求的格式时，抛出该异常**.

其中： `Invalid int: "0 "` 提示把 "0 " 转换成数字类型时出错了.

## 二、原因和解决方法

### 原因一：数字后面有空格
"0 "在0后面有空格，**在字符串转换成数字时应该去除空格**。
如：`int vale = Integer.parseInt(s.toString().trim());`
`toString()` 是转化为字符串的方法 ，`Trim()` 是去字符串两边空格的方法。


### 原因二：超出转换数值类型范围
用 `Integer.parseInt()` 转换字符时抛出 `NumberFormatException` 异常，把字符改短一点又没事
```
String   line3[1]= "8613719716 "; 
int   int1=java.lang.Integer.parseInt(line3[1]); 
```
以上是程序中的一小段，但是在运行的过程中总是抛出异常 
`Exception   in   thread   "main "   java.lang.NumberFormatException:   For   input   string:   "8613719716 " `

**原因分析**
int 类型存储范围是`-2,147,483,648 - 2,147,483,647`。用`System.out.println(Integer.MAX_VALUE);` 输出的是2147483647 。而 `String line3[1]= "8613719716 ";` 超过了这个最大的值。

**解决方案**
8613719716 根本无法直接使用 int 表示的，只能用 long ，如果更大了就得用到 BigInteger 。`Long.parseLong(String)`。


###  原因三：转换值类型没有考虑值为空的状况

把得到的edittext中的值转换为整数.
`startTime = Integer.parseInt(startTime_text.getEditableText().toString());`

 logcat 出现了如下错误. `java.lang.NumberFormatException: unable to parse '' as integer`

**原因分析**

如果 `startTime_text` 为空， Integer.parseInt就会试图把 "" 转换成 integer。这就是NumberFormatException出现的原因。所以在转换为int类型前需要判断startTime_edittext中是否为空。

**解决方案**

在使用 `startTime = Integer.parseInt(startTime_text.getEditableText().toString());`之前使用判断条件：

```
if(!startTime_text.getText().toString().equalsIgnoreCase("")) {
startTime = Integer.parseInt(startTime_text.getEditableText().toString());
}
```


### 原因四：由于进制不同

题主要做一个进制转换.并且限定范围为 30位的数 (1073741823) 或者(0111111111111111111111111111111). 问题出现在试图转换 111111111111111111111111111111的时候,出现 NumberFormatException. 

此代码是检查输入如果是二进制就转换为int型数值
```
if (checkNumber(input)) {
        try {
        number = Integer.parseInt(input);
        } catch (NumberFormatException ex) {
            log(ex.getMessage());
        }
    } else {
        toDecimal();
    }
```

这是检查 String的布尔返回值方法的代码.

```
private static boolean checkNumber(String input) {
    for (char c : input.toCharArray()) {
        if (!Character.isDigit(c)) {
            return false;
        }
    }
 
    return true;
}
```

出现异常:
```
java.lang.NumberFormatException: For input string: "111111111111111111111111111111"
```

**原因分析**
因为 `Integer.parseInt(String)` 默认是十进制.

所以需要使用 `Integer.parseInt(String, int)`并且指定要转换的n进制的数字的n。比如二进制是2.

**解决方案**
```
int value = Integer.parseInt(input, 2);
```
