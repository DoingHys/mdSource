[TOC]



## concurentHashMap实现原理

### 参考博客：（jdk7）

https://www.cnblogs.com/chengxiao/p/6842045.html

### 概述

ConcurrentHashMap是Java并发包中提供的一个线程安全且高效的HashMap实现（若对HashMap的实现原理还不甚了解，可参考我的另一篇文章**[HashMap实现原理及源码分析](http://www.cnblogs.com/chengxiao/p/6059914.html)**），ConcurrentHashMap在并发编程的场景中使用频率非常之高，本文就来分析下ConcurrentHashMap的实现原理，并对其实现原理进行分析（JDK1.7).

## 可重入锁







