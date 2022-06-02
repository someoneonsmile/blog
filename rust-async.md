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

  值到值的转换, 作用别 iter::map 差不多

- `then()`: (item) -> future<item>, stream::output: item

  值到异步闭包值的转换

- `buffered()`: stream<future> -> stream<future::output>

  future 到 future::output 的转换

- `collect()`: stream -> future

  流到 future 的转换

- `futures::stream::iter()`: (item) -> Poll::Ready(item)

  将普通迭代器包装成异步迭代器 (异步流)

- `futures::stream::for_each_concurrent()`: limit: Option<u32>, || async {} 实现异步逻辑

  内部用 futures_unordered 实现并发执行  
  因为流的编排 (如: map) 跟 正常迭代器的编排 (如: map)  
  都是 lazy 的, 所以即使前面的 future 是用 tokio::spawn 生成的  
  这里的 limit 也会生效

- `futures::stream::try_for_each_concurrent()`: limit: Option<u32>, || async {} 实现异步逻辑

  跟 try_for_each_concurrent 不同的是, 闭包的入参是 <item as Result>::output  
  返回值也是 Result 类型
