# Integer.bitCount 方法解读

```java
 public static int bitCount(int i) {
    // HD, Figure 5-2
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

整体分为两部：

- 将数值变成每两位表示这两位二进制中 1 的数量的二进制数

- 将这些数据加在一起

第一部(第一行代码)的转换逻辑:  

> 数值表示方法为 (低位 1) + (高位 1 * 2)  
> 个数表示方法为 (低位 1) + (高位 1)  
> 所以数值转换为个数的转换方法为: 数值 - (高位 1)

第二部的转换逻辑:

> 将相邻的两个两位二进制位数加和并放到加和的四个位上
> 将相邻的两个四位二进制位数加和并放到加和的八个位上
> 将相邻的两个八位二进制位数加和并放到加和的十六个位上
> 将相邻的两个十六位二进制位数加和并放到加和的三十二个位上

## 参考

[https://blog.csdn.net/u011497638/article/details/77947324](https://blog.csdn.net/u011497638/article/details/77947324)