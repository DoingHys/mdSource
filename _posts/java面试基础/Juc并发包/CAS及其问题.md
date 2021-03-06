# CAS

`CAS`即`Compare And Swap`的缩写，翻译成中文就是**比较并交换**，其作用是让CPU比较内存中某个值是否和预期的值相同，如果相同则将这个值更新为新值，不相同则不做更新，也就是CAS是**原子性**的操作(读和写两者同时具有原子性)，其实现方式是通过借助`C/C++`调用CPU指令完成的，所以效率很高，此外，他也是乐观锁的一种实现；



# CAS带来的ABA问题

在多线程场景下`CAS`会出现`ABA`问题，关于ABA问题这里简单科普下，例如有2个线程同时对同一个值(初始值为A)进行CAS操作，这三个线程如下

1. 线程1，期望值为A，欲更新的值为B
2. 线程2，期望值为A，欲更新的值为B

线程`1`抢先获得CPU时间片，而线程`2`因为其他原因阻塞了，线程`1`取值与期望的A值比较，发现相等然后将值更新为B，然后这个时候**出现了线程`3`，期望值为B，欲更新的值为A**，线程3取值与期望的值B比较，发现相等则将值更新为A，此时线程`2`从阻塞中恢复，并且获得了CPU时间片，这时候线程`2`取值与期望的值A比较，发现相等则将值更新为B，虽然线程`2`也完成了操作，但是线程`2`并不知道值已经经过了`A->B->A`的变化过程。

# ABA 问题引发的危害

小明在提款机，提取了50元，因为提款机问题，有两个线程，同时把余额从100变为50
 线程1（提款机）：获取当前值100，期望更新为50，
 线程2（提款机）：获取当前值100，期望更新为50，
 线程1成功执行，线程2某种原因block了，这时，某人给小明汇款50
 线程3（默认）：获取当前值50，期望更新为100，
 这时候线程3成功执行，余额变为100，
 线程2从Block中恢复，获取到的也是100，compare之后，继续更新余额为50！！！
 此时可以看到，实际余额应该为100（100-50+50），但是实际上变为了50（100-50+50-50）这就是ABA问题带来的成功提交。

 **解决方法**： 在变量前面加上版本号，每次变量更新的时候变量的**版本号都`+1`**，即`A->B->A`就变成了`1A->2B->3A`。



# 优点

`Java`利用`CAS`的乐观锁、原子性的特性高效解决了多线程的安全性问题，例如JDK1.8中的集合类`ConcurrentHashMap`、关键字`volatile`、`ReentrantLock`等。





**参考博客：**

https://juejin.im/post/5c87afa06fb9a049f1550b04