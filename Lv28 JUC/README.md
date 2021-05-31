## volatile 关键字-内存可见性
- 内存可见性（Memory Visibility）是指当某个线程正在使用对象状态而另一个线程在同时修改该状态，需要确保当一个线程修改了对象状态后，其他线程能够看到发生的状态变化。
## 原子变量-CAS算法

- CAS 是一种无锁的非阻塞算法的实现。
- CAS 包含了3 个操作数：
  - 需要读写的内存值V
  - 进行比较的值A
  - 拟写入的新值B
- 当且仅当V 的值等于A 时，CAS 通过原子方式用新值B 来更新V 的值，否则不会执行任何操作。

## ConcurrentHashMap 锁分段机制

- Java 5.0 在java.util.concurrent 包中提供了多种并发容器类来改进同步容器的性能。

- ConcurrentHashMap 同步容器类是Java 5 增加的一个线程安全的哈希表。对与多线程的操作，介于HashMap 与Hashtable 之间。内部采用“锁分段”机制替代Hashtable 的独占锁。进而提高性能。

- 此包还提供了设计用于多线程上下文中的Collection 实现：ConcurrentHashMap、ConcurrentSkipListMap、ConcurrentSkipListSet、CopyOnWriteArrayList 和CopyOnWriteArraySet。当期望许多线程访问一个给定collection 时，ConcurrentHashMap 通常优于同步的HashMap，ConcurrentSkipListMap 通常优于同步的TreeMap。当期望的读数和遍历远远大于列表的更新数时，CopyOnWriteArrayList 优于同步的ArrayList。

## CountDownLatch 闭锁

- CountDownLatch 一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。

- 闭锁可以延迟线程的进度直到其到达终止状态，闭锁可以用来确保某些活动直到其他活动都完成才继续执行：

  - 确保某个计算在其需要的所有资源都被初始化之后才继续执行;

  - 确保某个服务在其依赖的所有其他服务都已经启动之后才启动;

  - 等待直到某个操作所有参与者都准备就绪再继续执行。

## 实现Callable 接口

- Callable 接口类似于Runnable，两者都是为那些其实例可能被另一个线程执行的类设计的。但是Runnable 不会返回结果，并且无法抛出经过检查的异常。
- Callable 需要依赖FutureTask ，FutureTask 也可以用作闭锁。

## Lock 同步锁

- 在Java 5.0 之前，协调共享对象的访问时可以使用的机制只有synchronized 和volatile 。Java 5.0 后增加了一些新的机制，但并不是一种替代内置锁的方法，而是当内置锁不适用时，作为一种可选择的高级功能。
- ReentrantLock 实现了Lock 接口，并提供了与synchronized 相同的互斥性和内存可见性。但相较于synchronized 提供了更高的处理锁的灵活性

## Condition 控制线程通信

- Condition 接口描述了可能会与锁有关联的条件变量。这些变量在用法上与使用Object.wait 访问的隐式监视器类似，但提供了更强大的功能。需要特别指出的是，单个Lock 可能与多个Condition 对象关联。为了避免兼容性问题，Condition 方法的名称与对应的Object 版本中的不同。
- 在Condition 对象中，与wait、notify 和notifyAll 方法对应的分别是await、signal 和signalAll。
- Condition 实例实质上被绑定到一个锁上。要为特定Lock 实例获得Condition 实例，请使用其newCondition() 方法。

## 线程按序交替

## ReadWriteLock 读写锁

- ReadWriteLock 维护了一对相关的锁，一个用于只读操作，另一个用于写入操作。只要没有writer，读取锁可以由多个reader 线程同时保持。写入锁是独占的。。
- ReadWriteLock 读取操作通常不会改变共享资源，但执行写入操作时，必须独占方式来获取锁。对于读取操作占多数的数据结构。ReadWriteLock 能提供比独占锁更高的并发性。而对于只读的数据结构，其中包含的不变性可以完全不需要考虑加锁操作。

## 线程八锁

- 一个对象里面如果有多个synchronized方法，某一个时刻内，只要一个线程去调用其中的一个synchronized方法了，其它的线程都只能等待，换句话说，某一个时刻内，只能有唯一一个线程去访问这些synchronized方法
- 锁的是当前对象this，被锁定后，其它的线程都不能进入到当前对象的其它的synchronized方法
- 加个普通方法后发现和同步锁无关
- 换成两个对象后，不是同一把锁了，情况立刻变化。
- 都换成静态同步方法后，情况又变化
- 所有的非静态同步方法用的都是同一把锁——实例对象本身，也就是说如果一个实例对象的非静态同步方法获取锁后，该实例对象的其他非静态同步方法必须等待获取锁的方法释放锁后才能获取锁，可是别的实例对象的非静态同步方法因为跟该实例对象的非静态同步方法用的是不同的锁，所以毋须等待该实例对象已获取锁的非静态同步方法释放锁就可以获取他们自己的锁。
- 所有的静态同步方法用的也是同一把锁——类对象本身，这两把锁是两个不同的对象，所以静态同步方法与非静态同步方法之间是不会有竞态条件的。但是一旦一个静态同步方法获取锁后，其他的静态同步方法都必须等待该方法释放锁后才能获取锁，而不管是同一个实例对象的静态同步方法之间，还是不同的实例对象的静态同步方法之间，只要它们同一个类的实例对象！

## 线程池

- 第四种获取线程的方法：线程池，一个ExecutorService，它使用可能的几个池线程之一执行每个提交的任务，通常使用Executors 工厂方法配置。
- 线程池可以解决两个不同问题：由于减少了每个任务调用的开销，它们通常可以在执行大量异步任务时提供增强的性能，并且还可以提供绑定和管理资源（包括执行任务集时使用的线程）的方法。每个ThreadPoolExecutor 还维护着一些基本的统计数据，如完成的任务数。
- 为了便于跨大量上下文使用，此类提供了很多可调整的参数和扩展钩子(hook)。但是，强烈建议程序员使用较为方便的Executors 工厂方法：
  - Executors.newCachedThreadPool()（无界线程池，可以进行自动线程回收）
  - Executors.newFixedThreadPool(int)（固定大小线程池）
  - Executors.newSingleThreadExecutor()（单个后台线程）

## 线程调度

- ScheduledExecutorService
- 一个ExecutorService，可安排在给定的延迟后运行或定期执行的命令。

## ForkJoinPool 分支/合并框架工作窃取

- Fork/Join 框架：就是在必要的情况下，将一个大任务，进行拆分(fork)成若干个小任务（拆到不可再拆时），再将一个个的小任务运算的结果进行join 汇总。