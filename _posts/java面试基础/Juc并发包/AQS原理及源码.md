# 	1.谈谈你对AQS的理解

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

优质博客：https://www.cnblogs.com/waterystone/p/4920797.html

dou lea 博客：http://ifeve.com/doug-lea/


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

state:

```
    /**
     * The synchronization state.
     * 同步的状态
     */
         private volatile int state;
```

## 2.2 Node 节点结构

```
    static final class Node {
            /** Marker to indicate a node is waiting in shared mode 
            *表示节点在分享模式中等待
            */
        static final Node SHARED = new Node();
        
        /** Marker to indicate a node is waiting in exclusive mode
        *表示节点在等待独占模式
        */
        static final Node EXCLUSIVE = null;

        /** waitStatus value to indicate thread has cancelled 
        *waitStatus 的值表示线程已经被取消，需要踢出队列
        *因为中断或timeout，放弃资源竞争的标志,如果waitStatus等于此值，那么节点将不再会进		  *行争夺，并会被踢出队列
        */
        static final int CANCELLED =  1;
        
        /*
        * waitStatus value to indicate successor's thread needs unparking 
        * waitStatus 的值 表示线程需要释放
        * 需要唤醒队列中后继线程，如果waitStatus等于此值，那么当这个节点释放资源后，会通知			* 等待队列中的后继节点
        */
        static final int SIGNAL    = -1;
        
        /** 
        * waitStatus value to indicate thread is waiting on condition
        * waitStatus的值表示线程在condition中等待
        *线程等待触发条件,如果waitStatus等于此值，那么这个节点不会竞争资源，直到				*waitStatus被修改为0
        */
        static final int CONDITION = -2;
        
        /**
         * waitStatus value to indicate the next acquireShared should
         * unconditionally propagate
         * waitStatus 的值表示 共享模式无条件传播
         *将唤醒后继线程传递下去，这个状态的引入是为了完善和增强共享锁的唤醒机制。
         */
        static final int PROPAGATE = -3;
        
        //对应以上状态的等待状态标识位
        volatile int waitStatus;
        
        //前驱节点
        volatile Node prev;
        
        //后继节点
        volatile Node next;
        
        //线程
        volatile Thread thread;
        
        //等待队列中的后继节点
        Node nextWaiter;
        
        //在等待中的节点是否是处于共享模式
        final boolean isShared() {
            return nextWaiter == SHARED;
        }
        
        //获取前驱节点，如果不存在则返回位null
        final Node predecessor() throws NullPointerException {
            Node p = prev;
            if (p == null)
                throw new NullPointerException();
            else
                return p;
        }
        
        
        Node() {    // Used to establish initial head or SHARED marker
        }

		//addWaiter会调用此构造函数
        Node(Thread thread, Node mode) {     // Used by addWaiter
            this.nextWaiter = mode;
            this.thread = thread;
        }

		//Condition会用到此构造函数
        Node(Thread thread, int waitStatus) { // Used by Condition
            this.waitStatus = waitStatus;
            this.thread = thread;
        }
    }
```



## 2.3 模板方法

### 2.3.1 enq(final Node node)：

````
//阻塞尾部插入节点
private Node enq(final Node node) {
        for (;;) {
            Node t = tail;
            if (t == null) {
            // 如果尾节点t是null，代表链表没有元素，则通过CAS初始化一个头节点，并且把引用赋值给尾节点
                if (compareAndSetHead(new Node()))
                    tail = head;
            } else {
            // 若不为null,则把前驱节点赋值给 当前节点的{node.pre}
                node.prev = t;
                if (compareAndSetTail(t, node)) {//通过CAS把t的引用指向node
                    t.next = node;
                    return t;//for 循环的出口
                }
            }
        }
    }
    
这是节点进入同步队列的方法，其中的CAS操作compareAndSetTail(t, node)，作用为将tail的引用指向node，参数t中的内容是tail的旧引用对象地址，node是新节点的地址。再来看看AQS中compareAndSetTail(t, node)具体内容，

private final boolean compareAndSetTail(Node expect, Node update) {
        return unsafe.compareAndSwapObject(this, tailOffset, expect, update);
    }
tailOffset = unsafe.objectFieldOffset
                (AbstractQueuedSynchronizer.class.getDeclaredField("tail"));
这里的tailOffset，是通过unsafe类中的方法获取到的是“tail”变量相对于Java对象的“起始地址”的偏移量，即tailOffset可以理解为tail的地址。也就是说这里CAS操作更新的知识tail变量的地址上的数据，并不会对局部变量t上的值做修改，执行完compareAndSetTail(t, node)后，t还是指向原来的旧链尾，而tail指向了新的节点。

````

### 2.3.2 addWaiter()

```
	/**
    *创建并且入队当前线程和给定模式的 node节点
    private Node addWaiter(Node mode) {
        Node node = new Node(Thread.currentThread(), mode);
        // Try the fast path of enq; backup to full enq on failure
        Node pred = tail;
        if (pred != null) {
        //如果尾节点不为空，则将当前节点的前驱指向尾节点
            node.prev = pred;
            if (compareAndSetTail(pred, node)) {//     //CAS方式改变尾部节点的引用指向最新的node节点
                pred.next = node;//原来尾部节点的后驱指向当前的node节点
                return node;
            }
        }
        enq(node);
        return node;
    }
```

### 2.3.3 setHead(Node node)

```
   /**
   *设置头节点
   *
   **/
   private void setHead(Node node) {
        head = node;
        node.thread = null;
        node.prev = null;
    }
```








