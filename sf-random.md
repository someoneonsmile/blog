# 随机数产生转换

|    date    | tag  |
|    ---     | ---  |
| 2020-07-31 | 算法 |

## 问题描述

已知`random_m()`随机数生成器的范围是`[1, m]`, 求`random_n()`生成`[1, n]`范围的函数，`m < n && n <= m *m`

## 一般解

```c
int random_n()
{
    int val = 0 ;
    int t; // t为n最大倍数，且满足 t <= m * m
    do {
        val = m * (random_m() - 1) + random_m();
    } while (val > t);
    return 1 + (val - 1) % n;
}
```

## 分析

以 `m` 进制生成随机数, 每一个随机数代表一位, 取 `n` 最大倍数个数, 再除以倍数
