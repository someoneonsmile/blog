# 异步

## 实现方式

- 线程 (系统)
- 协程
- 异步 (单线程/多线程)

## 比较点

- 处理大量 IO 任务
- 数据流和错误传播 (回调是非线程控制流)
- 开销

## futures stream

异步流, (流的转换允许异步函数)

- `map()`: (item) -> item, stream::output: item

  值到值的转换

- `then()`: (item) -> future<item>, stream::output: item

  值到异步闭包值的转换

- `buffered`: stream<future> -> stream<future::output>

  future 到 future::output 的转换

- `collect`: stream -> future

  流到 future 的转换
