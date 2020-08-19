# 阿里面试答案

## JAVA 基础

1. JAVA 中的几种基本数据类型, 各自占用多少字节

    byte(1) short(2) int(4) long(8) float(4) double(8) boolean(1) char(2)

2. String 类能被继承吗, 为什么

    String 因为是 final 修饰的类不能被继承

3. String, StringBuffer, StringBuilder 的区别

    String 是不可变类, StringBuffer 及 StringBuilder 是可变类, StringBuffer 是线程安全的类,  
    不存在多线程竞争的情况下, StringBuilder 效率更高

4. ArrayList 和 LinkedList 有什么区别

    ArrayList 是基于数组的, LinkedList 是基于链表的, 查找是 ArrayList 效率更高,  
    在头部插入删除时, LinkedList 效率更高

5. 讲讲类的实例化顺序, 比如父类静态数据, 构造函数, 字段, 静态数据, 构造函数, 字段, 当 new 的时候, 他们的执行顺序

    加载父类到内存, 为父类静态字段开辟内存并进行默认初始化, 父类静态数据显示初始化, 父类静态初始化块  
    加载子类到内存, 为子类静态字段开辟内存并进行默认初始化, 显示初始化子类静态字段, 子类静态初始化块  
    父类变量初始化, 父类初始化块, 父类构造函数  
    子类变量初始化, 子类初始化块, 子类构造函数

6. 用过哪些 map 类, 都有什么区别, HashMap 是线程安全的吗, 并发下使用的是 map 是什么?
他们的内部原理是什么, 比如存储方式, hashcod, 扩容, 默认容量

    - 用过的 map

        HashMap, LinkedHashMap, TreeMap, ConcurrentHashMap, WreakHashMap

    - 内部原理

        [java 中 Map 有哪些实现类](https://blog.csdn.net/qq_30683329/article/details/80455779)
        HashMap 基于哈希表(数组加单向链表, 1.8 改用红黑树), LinkedHashMap 基于数组加双向链表,  
        ConcurrentHashMap 1.7 基于分段锁(segment 相当于 HashTable), 1.8 基于 cas 加 synchronize

7. 在自己的代码中, 如果创建一个 java.lang.String 类, 这个类是否可以被类加载器加载？为什么？

    不可以, java 的类加载机制是双亲委派模型, 类加载器

8. java 8 的特性

    lambda 表达式, 方法引用, 函数式接口, 接口默认方法、静态方法, Stream API, Optional 类, DateTime API

9. spring 的事物是怎么实现的？

    Spring 的事物, 是基于 Aop 实现的, 通过 TransactionInterceptor 拦截器, 实现事物的拦截  
    事物只能生效于 public 方法上, 自调用 (方法内部调用) 事物不会生效  
    解决方法, 通过其他 service 调用, 或通过 aspectj 实现代理事物, aspectj 是在编译时修改, 所以会生效  
    另外默认情况下, 事物在 controller 层不会生效, 因为 <tx:annoation-driven/>  
    只会查找和它在相同的应用上下文中定义的 bean 上面的 @Transactional 注解  
    [spring 事物自调用解决办法](https://blog.csdn.net/zy_281870667/article/details/77164312)

10. spring, spring mvc 父子容器

    spring 容器(上下文)创建完成之后, 当初始化 spring mvc 容器的时候, 就会将之前初始化的 spring 容器设置为它的父容器
    父子容器的关系:
      - 子容器能够访问父容器的资源(bean)

        如: Controller 可以注入 Service

      - 父容器不能访问子容器的资源(bean)

        如果将 Spring 容器和 Spring mvc 容器所扫描的包都配置成全部, 就会在子容器和父容器都有全部的自定义 bean.  
        当子容器中装载了 service 和 dao, 控制层就会使用子类所装载的 bean 去执行,  
        但是这里面的 service 是没有事物等功能的, 只是普通的 bean, 因此使用时会有不可预知的问题  
        @Value 只能在当前所在容器获取值

11. spring 动态代理的两种方式？

    jdk 动态代理, cglib 两方式, jdk 实现接口的方式实现代理,  
    cglib 通过运行时动态生成类的子类来代理, cglib 会更高效些  
    (对于单例或运用对象池技术, jdk 1.8 的动态代理也做了很大优化)

12. 双重判断锁为什么要加 volatile ?

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

    happens-before 原则:  
        1. 程序次序规则：一个线程内，按照代码顺序，书写在前面的操作先行发生于书写在后面的操作
        2. 锁定规则：一个unLock操作先行发生于后面对同一个锁额lock操作
        3. volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作
        4. 传递规则：如果操作A先行发生于操作B，而操作B又先行发生于操作C，则可以得出操作A先行发生于操作C

    MESI 缓存一致性协议

    主内存, 工作内存:
        Java内存模型规定所有的变量都是存在主存当中（类似于前面说的物理内存），每个线程都有自己的工作内存（类似于前面的高速缓存）。  
        线程对变量的所有操作都必须在工作内存中进行，而不能直接对主存进行操作。并且每个线程不能访问其他线程的工作内存。

    volatile 原理和实现机制:
        下面这段话摘自《深入理解Java虚拟机》:

    　　“观察加入volatile关键字和没有加入volatile关键字时所生成的汇编代码发现，加入volatile关键字时，会多出一个lock前缀指令”

    　　lock前缀指令实际上相当于一个内存屏障（也成内存栅栏），内存屏障会提供3个功能：

    　　1. 它确保指令重排序时不会把其后面的指令排到内存屏障之前的位置，也不会把前面的指令排到内存屏障的后面;  
    即在执行到内存屏障这句指令时，在它前面的操作已经全部完成；

    　　2. 它会强制将对缓存的修改操作立即写入主存；

    　　3. 如果是写操作，它会导致其他CPU中对应的缓存行无效。

    > [参见原文](https://www.cnblogs.com/dolphin0520/p/3920373.html)

13. cms 垃圾回收怎么解决内存碎片的?

    备用小知识:  

    启用 CMS (-XX:+UseConcMarkSweepGC)

    remark 阶段的内存范围是整个堆, 包含 young_gen 和 old_gen, 因存在跨代引用(新生代对象引用老年代对象)  
    所以 young_gen 也是 cms 的 gcroot, 如果年轻代有大量引用需要被扫描会让 remark 阶段耗时增加  
    remark 阶段耗时很长时可以在 remark 之前先做一次 ygc (-XX:+CMSScavengeBeforeRemark)

    (-XX:+CMSParallelRemarkEnabled) 减少 remark 阶段暂停时间, 启用并行 remark.  
    如果 remark 阶段暂停时间长, 可以启用这个参数  

    cms 老年代 GC 采用的是标记-清除算法, 为解决内存碎片问题, cms 提供以下参数:  
    -XX:+UseCMSCompactAtFullCollection  在 FGC 的时候对老年代内存压缩
    -XX:CMSFullGCsBeforeCompaction=0  代表多少次 FGC 后对老年代做压缩操作
    内存压缩触发时机: cms 并发 gc 失败(promotion failed, concurrent mode failure), 回退到 FGC.

    concurrent mode failure(并发失败):  
    这个异常发生在cms正在回收的时候.  
    执行 CMS GC 的过程中, 同时业务线程也在运行, 当年轻带空间满了, 执行 ygc 时, 需要将存活的
    对象放入到老年代, 而此时老年代空间不足, 这时CMS还没有机会回收老年带产生的, 或者
    在做Minor GC的时候, 新生代救助空间放不下, 需要放入老年代, 而老年代也放不下而产生的

    promotion failure(提升失败):
    这个异常发生在年轻代回收的时候.  
    在进行Minor GC时, Survivor Space放不下, 对象只能放入老年代, 而此时老年代也放不下造成的  
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

14. 设计模式的七个基本原则

    单一职责, 开放-关闭, 里氏替换, 依赖倒转, 接口隔离, 迪米特法则, 组合复用原则

15. 23种设计模式

    创建型:  
    简单工厂模式, 工厂模式, 抽象工厂模式, 单例模式, 生成器模式, 原型模式, 建造者模式

    结构型:  
    外观模式, 适配器模式, 桥接模式, 代理模式, 装饰者模式, 享元模式

    行为型:  
    职责链模式, 命令模式, 解释器模式, 迭代器模式, 中介者模式, 备忘录模式, 观察者模式, 状态模式, 策略模式, 模板方法模式, 访问者模式
