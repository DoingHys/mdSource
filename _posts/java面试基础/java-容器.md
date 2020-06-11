[TOC]



# ArrayList

## 1.概述：

数据底层是数组，线程不安全；实现了 RandomAccess 接口，标记支持随机访问；

### 1.1 RandomAccess的作用

RandomAccess 是一个标志接口，表明实现这个这个接口的 List 集合是支持快速随机访问的。

如果是实现了这个接口的 **List**，那么使用for循环的方式获取数据会优于用迭代器获取数据。

###  **1.2 ArrayList** **与** **Vector** **区别**

Vector类的所有方法都是同步的。可以由两个线程安全地访问一个Vector对象；

但是一个线程访问Vector的话代码要在同步操作上耗费大量的时间。

Arraylist不是同步的，所以在不需要保证线程安全时时建议使用Arraylist。

