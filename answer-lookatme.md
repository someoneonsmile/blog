---
title: 面试
date: 2021-02-21
---

## JAVA 中的几种基本数据类型, 各自占用多少字节

byte(1) short(2) int(4) long(8) float(4) double(8) boolean(1) char(2)

---

## String 类能被继承吗, 为什么

String 因为是 final 修饰的类不能被继承

---

## String, StringBuffer, StringBuilder 的区别

String 是不可变类, StringBuffer 及 StringBuilder 是可变类, StringBuffer 是线程安全的类,
不存在多线程竞争的情况下, StringBuilder 效率更高

---

## ArrayList 和 LinkedList 有什么区别

ArrayList 是基于数组的, LinkedList 是基于链表的, 查找是 ArrayList 效率更高,
在头部插入删除时, LinkedList 效率更高

---

## 讲讲类的实例化顺序, 比如父类静态数据, 构造函数, 字段, 静态数据, 构造函数, 字段, 当 new 的时候, 他们的执行顺序

加载父类到内存, 为父类静态字段开辟内存并进行默认初始化, 父类静态数据显示初始化, 父类静态初始化块  
加载子类到内存, 为子类静态字段开辟内存并进行默认初始化, 显示初始化子类静态字段, 子类静态初始化块  
父类变量初始化, 父类初始化块, 父类构造函数  
子类变量初始化, 子类初始化块, 子类构造函数

---

## 用过哪些 map 类, 都有什么区别, HashMap 是线程安全的吗, 并发下使用的是 map 是什么? 他们的内部原理是什么, 比如存储方式, hashcod, 扩容, 默认容量

- 用过的 map

HashMap, LinkedHashMap, TreeMap, ConcurrentHashMap, WreakHashMap

- 内部原理

[java 中 Map 有哪些实现类](https://blog.csdn.net/qq_30683329/article/details/80455779)
HashMap 基于哈希表(数组加单向链表, 1.8 改用红黑树), LinkedHashMap 基于数组加双向链表,
ConcurrentHashMap 1.7 基于分段锁(segment 相当于 HashTable), 1.8 基于 cas 加 synchronize

---

## 在自己的代码中, 如果创建一个 java.lang.String 类, 这个类是否可以被类加载器加载？为什么？

不可以, java 的类加载机制是双亲委派模型, 类加载器

---

## java 8 的特性

lambda 表达式, 方法引用, 函数式接口, 接口默认方法、静态方法, Stream API, Optional 类, DateTime API

---

## spring 的事物是怎么实现的？

Spring 的事物, 是基于 Aop 实现的, 通过 TransactionInterceptor 拦截器, 实现事物的拦截

事物只能生效于 public 方法上, 自调用 (方法内部调用) 事物不会生效

解决方法, 通过其他 service 调用, 或通过 aspectj 实现代理事物, aspectj 是在编译时修改, 所以会生效

另外默认情况下, 事物在 controller 层不会生效, 因为 <tx:annoation-driven/>

只会查找和它在相同的应用上下文中定义的 bean 上面的 @Transactional 注解

- [spring 事物自调用解决办法](https://blog.csdn.net/zy_281870667/article/details/77164312)

---

## spring, spring mvc 父子容器

spring 容器(上下文)创建完成之后, 当初始化 spring mvc 容器的时候, 就会将之前初始化的 spring 容器设置为它的父容器

父子容器的关系:

- 子容器能够访问父容器的资源(bean)

如: Controller 可以注入 Service

- 父容器不能访问子容器的资源(bean)

如果将 Spring 容器和 Spring mvc 容器所扫描的包都配置成全部, 就会在子容器和父容器都有全部的自定义 bean.

当子容器中装载了 service 和 dao, 控制层就会使用子类所装载的 bean 去执行,

但是这里面的 service 是没有事物等功能的, 只是普通的 bean, 因此使用时会有不可预知的问题

@Value 只能在当前所在容器获取值

---

## spring 动态代理的两种方式？

jdk 动态代理, cglib 两方式, jdk 实现接口的方式实现代理,

cglib 通过运行时动态生成类的子类来代理, cglib 会更高效些

(对于单例或运用对象池技术, jdk 1.8 的动态代理也做了很大优化)

---

## 双重判断锁为什么要加 volatile ?

volatile 作用:

1. 保证内存可见性. 使用 volatile 定义的变量将会保证对所有线程的可见性
2. 禁止指令重排序优化

new 实例背后的指令:

问题在于 `Cache c = new Cache()` 这行代码并不是一个原子指令

创建一个对象实例可以分为三步:

1. 分配对象内存
2. 调用构造器方法, 执行初始化
3. 将对象引用赋值给变量

虚拟机实际运行时, 以上指令可能发生重排序, 实例执行顺序可能为 132, 将导致其它线程访问到未初始化的对象

### happens-before 原则

1. 程序次序规则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作
2. 锁定规则：一个 unLock 操作先行发生于后面对同一个锁额 lock 操作
3. volatile 变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作
4. 传递规则：如果操作 A 先行发生于操作 B，而操作 B 又先行发生于操作 C，则可以得出操作 A 先行发生于操作 C

### MESI 缓存一致性协议

主内存, 工作内存:

Java 内存模型规定所有的变量都是存在主存当中（类似于前面说的物理内存），每个线程都有自己的工作内存（类似于前面的高速缓存）。

线程对变量的所有操作都必须在工作内存中进行，而不能直接对主存进行操作。并且每个线程不能访问其他线程的工作内存。

### volatile 原理和实现机制

下面这段话摘自《深入理解 Java 虚拟机》:

观察加入 volatile 关键字和没有加入 volatile 关键字时所生成的汇编代码发现，加入 volatile 关键字时，会多出一个 lock 前缀指令”

lock 前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供 3 个功能：

1. 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面.  
   即在执行到内存屏障这句指令时，在它前面的操作已经全部完成

2. 它会强制将对缓存的修改操作立即写入主存.

3. 如果是写操作，它会导致其他 CPU 中对应的缓存行无效.

- [参见原文](https://www.cnblogs.com/dolphin0520/p/3920373.html)

---

## cms 垃圾回收怎么解决内存碎片的?

备用小知识:

启用 CMS (-XX:+UseConcMarkSweepGC)

remark 阶段的内存范围是整个堆, 包含 young_gen 和 old_gen, 因存在跨代引用(新生代对象引用老年代对象)
所以 young_gen 也是 cms 的 gcroot, 如果年轻代有大量引用需要被扫描会让 remark 阶段耗时增加
remark 阶段耗时很长时可以在 remark 之前先做一次 ygc (-XX:+CMSScavengeBeforeRemark)

(-XX:+CMSParallelRemarkEnabled) 减少 remark 阶段暂停时间, 启用并行 remark.
如果 remark 阶段暂停时间长, 可以启用这个参数

cms 老年代 GC 采用的是标记-清除算法, 为解决内存碎片问题, cms 提供以下参数:
-XX:+UseCMSCompactAtFullCollection 在 FGC 的时候对老年代内存压缩
-XX:CMSFullGCsBeforeCompaction=0 代表多少次 FGC 后对老年代做压缩操作
内存压缩触发时机: cms 并发 gc 失败(promotion failed, concurrent mode failure), 回退到 FGC.

concurrent mode failure(并发失败):
这个异常发生在 cms 正在回收的时候.
执行 CMS GC 的过程中, 同时业务线程也在运行, 当年轻带空间满了, 执行 ygc 时, 需要将存活的
对象放入到老年代, 而此时老年代空间不足, 这时 CMS 还没有机会回收老年带产生的, 或者
在做 Minor GC 的时候, 新生代救助空间放不下, 需要放入老年代, 而老年代也放不下而产生的

promotion failure(提升失败):
这个异常发生在年轻代回收的时候.
在进行 Minor GC 时, Survivor Space 放不下, 对象只能放入老年代, 而此时老年代也放不下造成的
多数是由于老年带有足够的空闲空间, 但是由于碎片较多, 新生代要转移到老年带的对象比较大
找不到一段连续区域存放这个对象导致的

过早提升与提升失败
在 Minor GC 过程中, Survivor Unused 可能不足以容纳 Eden 和另一个 Survivor 中的存活对象
那么多余的将被移到老年代, 称为过早提升(Premature Promotion)
这会导致老年代中短期存活对象的增长, 可能会引发严重的性能问题.
再进一步, 如果老年代满了, Minor GC 后会进行 Full GC, 这将导致遍历整个堆, 称为提升失败(Promotion Failure)

早提升原因:

- Survivor 空间太小, 容纳不下全部的运行时短生命周期对象, 解决办法: 调大 Survivor
- 对象太大, Survivor 和 Eden 没有足够大的空间来存放这些大对象

提升失败原因:

- 老年代空闲空间不够用了
- 老年代空间空间虽然很多, 但碎片太多, 没有连续空闲空间存放该对象

初始标记:

- GC Roots 能直接关联到的对象
- 遍历被新生代存活对象所引用的老年代对象

GC Roots 对象包括如下几种:

- 虚拟机栈中引用的对象
- 方法区类静态属性引用的对象
- 方法区中常量引用的对象
- 本地方法栈中 JNI 的引用对象

MAT 分析工具中 shallow heap 跟 retained heap 区别:
shallow heap: 对象实际占用堆的大小
retained heap: 回收这个对象可以回收多少内存 (+ 因这个对象被回收, 而回收的对象)

参考:

- [CMS 垃圾回收器](https://juejin.im/post/6844903782107578382)
- [CMS 详解](https://juejin.im/post/6844903970142421005)
- [CMS GC 的七个阶段](https://juejin.im/post/6844903740864987149)

---

## 设计模式的七个基本原则

单一职责, 开放-关闭, 里氏替换, 依赖倒转, 接口隔离, 迪米特法则, 组合复用原则

### 23 种设计模式

创建型:
简单工厂模式, 工厂模式, 抽象工厂模式, 单例模式, 生成器模式, 原型模式, 建造者模式

结构型:
外观模式, 适配器模式, 桥接模式, 代理模式, 装饰者模式, 享元模式

行为型:
职责链模式, 命令模式, 解释器模式, 迭代器模式, 中介者模式, 备忘录模式, 观察者模式, 状态模式, 策略模式, 模板方法模式, 访问者模式

举例:

- 简单工厂: guava Maps.newHashMap
- DelayQueue 内部组合 PriorityQueue

---

## AQS 相关

### 锁有哪些

ReentrantLock: 可重入锁, ReentrantReadWriteLock: 可重入读写
锁, StampedLock: 乐观读锁, 解决读多写少, 写饥饿问题

### 锁分类

公平/非公平, 独占/共享, 乐观/悲观(乐观读锁不与写锁互斥)

---

## spring boot 原理

### 自动配置

@SpringBootApplication 是个复合注解, 其包括:

- @SpringBootConfiguration (同 @Configuration)
- @EnableAutoConfiguration
- @ComponentScan

@EnableAutoConfiguration 由这两个注解组成:

- @AutoConfigurationPackage
- @Import(AutoConfigurationImportSelector.class)

@AutoConfigurationPackage 是由 @Import(AutoConfigurationPackages.Registrar.class) 组成  
@Import 注解可以导入类到 Spring Ioc 中, 对 ImportSelector, ImportBeanDefinitionRegistrar  
有特殊处理, 对 ImportSelector 会导入它返回的所有类, ImportBeanDefinitionRegistrar 会  
将 **添加该注解的类所在的 package** 作为 **自动配置 package** 进行管理, 让包中的类及子包  
中的类能被自动扫描到 Spring Ioc 中.

- [深入 spring boot 原理](https://www.cnblogs.com/hjwublog/p/10332042.html)
- [Spring @Import 机制](https://www.jianshu.com/p/3c5922ec3686)

---

## java 类加载机制

加载过程:

- 加载
- 链接
  - 验证
  - 准备
  - 解析
- 初始化

何时开始类的初始化?

主动引用:

- 创建类实例
- 访问类的静态变量(除常量)
- 访问类的静态方法
- Class.forName
- 初始化一个类时, 父类还未初始化, 先初始化父类
- 虚拟机启动时, 定义 main 方法的类先启动

### Class.forName 跟 ClassLoader.load 有什么区别?

Class.forName 会执行初始化, ClassLoader.load 不会执行初始化

能不能自己写个类叫 java.lang.String?

不能 defineClass -> preDifineClass 会检验是不是以 `java.` 开头, 会抛 SecurityException 异常
且 defineClass 为 `final` 不能被重写

自定义 ClassLoader 打破双亲委派模型?

一般自定义 ClassLoader 重写 `findClass` 方法, 打破双亲委派模型需要重写 `loadClass` 方法

- [类加载机制](https://juejin.im/post/6844903564804882445#heading-4)

---

## mysql mvcc, gap 锁, 解决快照读, 当前读

- [mvcc 和 间隙读](https://blog.csdn.net/wxy941011/article/details/79604704)

---

## mysql explain 执行计划分析

- [mysql 性能优化神器 explain 使用分析](https://segmentfault.com/a/1190000008131735)

---

## 如何查找慢 sql

TODO:

---

## kafka 为什么快？

- 磁盘顺序读写
- 利用操作系统 `page cache`
- 零拷贝
- 分区分段 + 索引
- 批量读写
- 批量压缩

### kafka 分布式情况下，如何保证消息顺序？

Kafka 分布式的单位是 `partition`，同一个 `partition` 用一个 `write ahead log` 组织，所以可以保证 FIFO 的顺序。

不同 `partition` 之间不能保证顺序。

但是绝大多数用户都可以通过 `message key` 来定义，因为同一个 `key` 的 `message` 可以保证只发送到同一个 `partition`

比如说 `key` 是 `user id`，`table row id` 等等

所以同一个 `user` 或者同一个 `record` 的消息永远只会发送到同一个 `partition` 上，保证了同一个 `user` 或 `record` 的顺序

当然，如果你有 `key skewness` 就有些麻烦，需要特殊处理

参考：

- [kafka 为什么吞吐量大、速度快？](https://zhuanlan.zhihu.com/p/120967989)
- [kafka 消息顺序](https://www.zhihu.com/question/266390197)

---

## 第三方支付流程

---

## 商城下订单到支付基本流程

---

## mysql undo log

---

## epoll

---

## https 加密流程

---

## elasticsearch 防止脑裂

配置最小包含 master 节点数才可以选举

`discovery.zen.minimum_master_nodes: N/2 + 1`

---

## raft 协议

---

## 缓存雪崩，击穿怎么处理

---

## epoll 更高效的原因

1. select 和 poll 的动作基本一致，只是 poll 采用链表来进行文件描述符的存储，而 select 采用 fd 标注位来存放，所以 select 会受到最大连接数的限制，而 poll 不会。
2. select、poll、epoll 虽然都会返回就绪的文件描述符数量。但是 select 和 poll 并不会明确指出是哪些文件描述符就绪，而 epoll 会。造成的区别就是，系统调用返回后，调用 select 和 poll 的程序需要遍历监听的整个文件描述符找到是谁处于就绪，而 epoll 则直接处理即可。
3. select、poll 都需要将有关文件描述符的数据结构拷贝进内核，最后再拷贝出来。而 epoll 创建的有关文件描述符的数据结构本身就存于内核态中，系统调用返回时利用 mmap()文件映射内存加速与内核空间的消息传递：即 epoll 使用 mmap 减少复制开销。
4. select、poll 采用轮询的方式来检查文件描述符是否处于就绪态，而 epoll 采用回调机制。造成的结果就是，随着 fd 的增加，select 和 poll 的效率会线性降低，而 epoll 不会受到太大影响，除非活跃的 socket 很多。
5. epoll 的边缘触发模式效率高，系统不会充斥大量不关心的就绪文件描述符

[彻底搞懂 epoll 高效的原因](https://www.jianshu.com/p/31cdfd6f5a48)

---

## redis 缓存淘汰策略

LRU(最久未使用), random, ttl(离过期时间最近), LFU(使用频率最少)

LFU: 16 位时钟, 8 位频率(最多 255), redis 并没有使用线性上升的方式, 而是通过
一个复杂的公式, 通过配置两个参数来调整数据的递增速度

```c
uint8_t LFULogIncr(uint8_t counter) {
    if (counter == 255) return 255;
    double r = (double)rand()/RAND_MAX;
    double baseval = counter - LFU_INIT_VAL;
    if (baseval < 0) baseval = 0;
    double p = 1.0/(baseval*server.lfu_log_factor+1);
    if (r < p) counter++;
    return counter;
}
```

- [redis 的缓存淘汰策略 LRU 和 LFU](https://www.jianshu.com/p/c8aeb3eee6bc)

---

## redis 热点 key, 怎么查找, 怎么处理

TODO:

---

## redis 持久化策略

- [持久化策略浅析](https://segmentfault.com/a/1190000009537768)

---

## redis rehash

[原文链接](https://zhuanlan.zhihu.com/p/358366217)
