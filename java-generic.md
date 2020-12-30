# java 泛型

|    date    | tag  |
|    ---     | ---  |
| 2020-11-16 | 泛型 |

## 上下边界泛型

```java
List<? extends SomeType>
List<? super SomeType>
```

> 不要被 `extends` `super` 所迷惑

- 容器里面都是 `SomeType` 的子类

- `extends` 只能读, 可以用 `SomeType` 及其父类接收

- `super` 只能写, 只能写入 `SomeType` 及其子类

- `extends` 定义了容器泛型的上边界, 边界以下都是不确定的, 边界以上的对象可以接收

- `super` 定义了窗口泛型的下边界, 边界以上都是不确定的, 边界以下的对象才可以写入

- 泛型不能直接执行类的静态方法, 泛型只是作为类型约束

