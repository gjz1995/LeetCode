

# StringSummary



- 数字转字符串：`Integer.toString(a);`
- StringBuilder 转 Int，需要首先使用 `.toString()`方法转换为 String，然后使用 `Integer.paseInt(XX)`转换为 Int 类型。
- 字符串要是排序，需要先使用 `XXX.toCharArray()`转换为字符数组，然后使用 `Arrays.sort()` 进行排序。

- 数组转为字符串：`String.valueOf(XXX);`