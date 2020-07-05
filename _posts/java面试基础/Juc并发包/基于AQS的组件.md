# AQS之CountDownLatch

图解：

![image-20200705120020448](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200705120020448.png)

概述：是一个同步辅助类，通过它可以完成线程类似阻塞，换句话说就是一个线程或者多个线程同时等待直到其他线程的操作完成；countDownLatch 用了一个给定的计数器进行初始化，该计数器的操作是原子操作，同时只能有一个线程去操作该计数器；初始化以后当该线程调用await（）方法以后，其他线程通过执行countDown（）方法可以让计数器减一，当计数器减少到0的时候，当前线程可以被唤醒继续执行后续程序；



# AQS之Semaphore

图解：

![image-20200705134447045](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200705134447045.png)

概述：

 