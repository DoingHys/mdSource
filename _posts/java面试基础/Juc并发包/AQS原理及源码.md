# 1.谈谈你对AQS的理解

**概念：AbstractQueuedSychronizer  ** 简称AQS；自从jdk1.5开始，jdk引入了并发包juc，juc极大的提高了java的并发性能，而aqs被认为是juc的核心，它提供了一个FIFO的队列 ，它是能够用来构建锁或同步器的基础框架；它底层是使用双向链表（Sync queue），是队列的一种实现；因此我们也可以把它当作是一种队列；该队列包含head节点和tail节点，head节点主要用来后续的调度；除此，他还包含一个Condition Queue,它不是必须的，是一个单向链表；只有aqs使用 到Condition的时候才会启动这个单向链表，而且可能会有多个condition queue（ps：需要对这个数据结构有大致的印象）

图解：

![image-20200705110758456](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200705110758456.png)



***

**AQS的设计：**

* 使用Node实现FIFO队列，可以用于构建锁和其他同步装置 的基础框架

* 利用了一个int类型的状态

  在aqs类中，有一个叫做state的成员变量

  state=0 表示没有线程获取锁; 

  state=1，已经有线程获取了锁；

  state>1,重入锁的数量

* 使用方法是继承

  AQS的设计是基于模板方法的，使用它需要继承这个AQS,并且复写其中的方法

* 子类通过继承并通过实现它的方法管理其状态{acquire和release}的方法操纵状态

* 可以同时实现排它锁和共享锁模式（独占、共享）

  它的所有子类要么实现了独占、要么实现共享 API，而不会同时使用两套 API；

***

**AQS实现的大致思路**：

首先，AQS内部维护了一个 CLH 队列来管理锁；线程会首先尝试获取锁， 如果失败，就让线程以及等待状态等信息保存一个node节点加入到一个叫做Sycn queue的同步队列里；接着会不断的循环尝试获取锁，条件是，如果当前节点为head 则后继下个尝试，如果失败就会阻塞自己知道自己被唤醒 ；而持有锁的线程释放锁的时候，会唤醒队列中的后继线程；

基于AQS的设计和思路，jdk提供了许多基于AQS的子类，就是我们常用的那些同步的组件；

AQS 同步的组件：

* CountDownLatch

  闭锁，通过一个技术来保证线程是否需要一直阻塞 

* Semaphore

  能够控制同一时间线程并发的数目

* CyclicBarrier 

  和CountDown Latch很像，都能阻塞进程

* **ReentrankLock**

* Condition

* FutureTask



# 2.AQS 源码理解

参考博客：https://www.cnblogs.com/micrari/p/6937995.html

## 2.1AQS 内部变量

**head节点：**

```
/**
*等待队列的头节点，会被延迟初始化；除了被初始化赋值以外，它只能通过setHead()方法去给它赋值；注意：如果头节点存在，它的waitStatus 状态被确保不能够是CANCELED(被移除队列的标志);
**/
private transient volatile Node head;
```

tail节点;

```
    /**
     * Tail of the wait queue, lazily initialized.  Modified only via
     * method enq to add new wait node.
     *等待队列的尾节点，延迟初始化，只能通过方法enq()去增加新的等待节点
     */
    private transient volatile Node tail;
```







