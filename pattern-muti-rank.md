# 多阶梯递进模式

|    data    |   tag   |
|    ---     |   ---   |
| 2021-01-10 | pattern |

## 描述

存在多个阶段递进依赖, 有相互转化关系

当前的阶段为最高阶段, 大于当前阶段的都清空

## 模式

- 抽象

```python
def rank(exist, latest):
    if not latest.hight :
        // check
        check(exist.middle is not None)

        // set from curreet
        latest.hight_relate = something

        // 归一
        latest.middle = exist.middle
        exist.middle = None
    if not latest.middle :
        // check
        check(exist.middle is None)
        check(exist.low is not None)

        // set from exist or current
        latest.middle_relate = exist.middle_relate ?? something

        // 归一
        latest.low = exist.low
        exist.low = None

    // if not latest.low :
    // check
    check(exist.low is None)
    latest.low_relate = exist.low_relate ?? something

```

- 实际

```java
/** 校验并初始化 */
public void initScore(SignUp exist, SignUp signUp) {
    SysUser sessionUser = (SysUser) SecurityUtils.getSubject().getSession().getAttribute(Constants.SESSION_ATTRIBUTE_USER);
    signUp.setReviewerId(sessionUser.getUserId());
    // 非审核通过打分清空
    if (ObjectUtil.notEqual(signUp.getReviewStatus(), ReviewStatusEnum.ONE.getCode())) {
        clearScore(signUp);
        return;
    }
    if (ObjectUtil.isNotNull(signUp.getThirdScore())) {
        Preconditions.checkArgument(ObjectUtil.isNotNull(exist.getSecondScore()), "第二轮还没打分");
        signUp.setThirdCheckName(sessionUser.getUserName());
        signUp.setSecondScore(exist.getSecondScore());
        exist.setSecondScore(null);
    }
    if (ObjectUtil.isNotNull(signUp.getSecondScore())) {
        Preconditions.checkArgument(ObjectUtil.isNull(exist.getSecondScore()), "第二轮已打过分数");
        Preconditions.checkArgument(ObjectUtil.isNotNull(exist.getFirstScore()), "请先进行第一轮打分");
        signUp.setSecondCheckName(ObjectUtil.defaultIfBlank(exist.getSecondCheckName(), sessionUser.getUserName()));
        signUp.setFirstScore(exist.getFirstScore());
        exist.setFirstScore(null);
    }
    if (ObjectUtil.isNotNull(signUp.getFirstScore())) {
        Preconditions.checkArgument(ObjectUtil.isNull(exist.getFirstScore()), "第一轮已打过分数");
        signUp.setFirstCheckName(ObjectUtil.defaultIfBlank(exist.getFirstCheckName(), sessionUser.getUserName()));
    }
}
```
