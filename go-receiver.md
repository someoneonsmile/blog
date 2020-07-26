# go 中方法的接收者是值或是指针的区别

> date: 2020-07-26

- 值类型调用接收者是值类型的方法

不会改变结构体内部的值

> go 内部处理时是值拷贝

- 值类型调用接收者是指针类型的方法

可以改变结构体内部的值  

> go 内部可能是在调用的时候把值类型的指针传给了方法

- 指针类型调用接收者是值类型的方法

不会改变结构体内部的值

> 取指针类型的值, 并拷贝传给方法

- 指针类型调用接收者是指针类型的方法

会改变结构体内部的值

> 都是指针类型不需要做转换

## 小结

> 接收者是值类型会做值拷贝, 接收者是指针类型时会取值类型的指针

```go
func (t T) M() {

}
func (t *T) M() {

}
// 相当于
func M(t T) {

}
func M(t *T) {

}
```

## 传给接口会怎样

含有接收者是指针类型的接口方法, 实现者 `值类型` 赋值给 `接口类型` 时会报错  
因为值类型没有实现指针接收者的方法  
~~因为赋值时发生的是 `值拷贝`, 调用接口的接收者是指针类型的方法时, 不会改变原结构体的内部值~~  
解决方法: 把 `值类型的指针` 赋值给 `接口类型`

> - 如果实现了接收者是值类型的方法，会隐含地也实现了接收者是指针类型的方法
> - 值类型跟指针类型可以看做两种类型, 实没实现接口要看该类型有没有实现接口的所有方法

## 嵌套类型

### 嵌套值类型

```go
type S struct {
    T
}

t := T{"t"}
s := S{t}
```

此时接收者为指针类型的方法可以改变 `s` 的值

> 因为 `s.M()` 相当于 `M(t)` 而不是 `M(s)`
> 调用时会取 `S内部T`

改变不了 `t` 的值  

> 因为 S 嵌套的是值类型的 `T` 所以 `S{t}` 时将 `t` 进行了值拷贝  
> `s` 与 `t` 已无关系

传给接口时同样也会报错

> 因为传给接口时也是发生的值拷贝, 值类型没有指针接收者的方法, 所以一样没有实现接口的方法  
> 解决方法：将 `s` 的指针 赋值给 接口类型

### 嵌套指针类型

```go
type S struct {
    *T
}
t := T{"t"}
s := S{&t}
```

会改变 `s` 值, 也会改变 `t` 值  
传给接口时可以编译通过

> 因为虽然传给 接口类型 的也是值拷贝, 但其持有的 `t` 为指针类型, 可以改变结构体内部的值

## 结论

> - 凡是赋值给 指针类型的 都可以改变原值  
> - 凡是赋值给 值类型的 都是值拷贝, 如果内部有指针类型, 也可以改变内部指针类型内部值
> - 嵌套值类型 嵌套时会发生值拷贝
> - 嵌套指针类型 嵌套时拷贝的是指针
> - 如果实现了接收者是值类型的方法，会隐含地也实现了接收者是指针类型的方法
> - 值类型跟指针类型可以看做两种类型, 实没实现接口要看该类型有没有实现接口的所有方法

## 测试

```go
type T struct {
    Name string
}

type S struct {
    T
}

func (t T) M1() {
    t.Name = "M1"
}

func (t *T) M2() {
    t.Name = "M2"
}

func TestReceiver(_ *testing.T) {

    t := T{"T"}
    fmt.Println("M1 before:", t.Name)
    t.M1()
    fmt.Println("M1 after:", t.Name)
    fmt.Println("M2 before:", t.Name)
    t.M2()
    fmt.Println("M2 after:", t.Name)

    fmt.Println(strings.Repeat("*", 20))
    t = T{"T"}
    s := S{t}
    fmt.Println("M1 before:", s.Name)
    s.M1()
    fmt.Println("M1 after:", s.Name)
    fmt.Println("M2 before:", s.Name)
    s.M2()
    fmt.Println("M2 after:", s.Name)
    fmt.Println("t:", t.Name)
}
```

输出结果:

```go
M1 before: T
M1 after: T
M2 before: T
M2 after: M2
********************
M1 before: T
M1 after: T
M2 before: T
M2 after: M2
t: T
```

## 原文

[https://blog.csdn.net/u013790019/article/details/45397287](https://blog.csdn.net/u013790019/article/details/45397287)

发现一个更好的系列文章  
[值接收者和指针接收者的区别](https://qcrao91.gitbook.io/go/interface/zhi-jie-shou-zhe-he-zhi-zhen-jie-shou-zhe-de-qu-bie)
