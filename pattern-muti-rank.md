# 多阶梯递进模式

|    data    |   tag   |
|    ---     |   ---   |
| 2021-01-10 | pattern |

## 描述

存在多个阶段递进依赖，有相互转化关系

例：当前的阶段为最高阶段，大于当前阶段的都清空

## 模式

```python
def rank(exist, latest):
    if not latest.hight :
        // check
        check(exist.middle is not None, 'middle not exist')

        // set from curreet
        latest.hight_relate = something

        // 归一
        latest.middle = exist.middle
        exist.middle = None
    if not latest.middle :
        // check
        check(exist.middle is None, 'middle has exist')
        check(exist.low is not None, 'low not exist')

        // set from exist or current
        latest.middle_relate = exist.middle_relate ?? something

        // 归一
        latest.low = exist.low
        exist.low = None

    // if not latest.low :
    // check
    check(exist.low is None, 'low has exist')
    latest.low_relate = exist.low_relate ?? something
```
