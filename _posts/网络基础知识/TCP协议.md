# TCP协议简介

* 面向连接的、可靠的、基于字节流的传输层通信协议
* 将应用层的数据流分割成报文段并发送给目标点的TCP层
* 数据包都有序号，对方收到则发送ACK确认，未收到则重传
* 使用校验和来 校验数据 在传输过程中是否有误



# TCP报文头

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20180811081238887?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3plcWkxOTkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

逐个分析：
16位源端口号和16位目的端口号。
32位序号：一次TCP通信过程中某一个传输方向上的字节流的每个字节的编号，通过这个来确认发送的数据有序，比如现在序列号为1000，发送了1000，下一个序列号就是2000。
32位确认号：用来响应TCP报文段，给收到的TCP报文段的序号加1，三握时还要携带自己的序号。
4位头部长度：标识该TCP头部有多少个4字节，共表示最长15*4=60字节。同IP头部。
6位保留。6位标志。URG（紧急指针是否有效）ACK（表示确认号是否有效）PSH（提示接收端应用程序应该立即从TCP接收缓冲区读走数据）RST（表示要求对方重新建立连接）SYN（表示请求建立一个连接）FIN（表示通知对方本端要关闭连接）
16位窗口大小：TCP流量控制的一个手段，用来告诉对端TCP缓冲区还能容纳多少字节。
16位校验和：由发送端填充，接收端对报文段执行CRC算法以检验TCP报文段在传输中是否损坏。
16位紧急指针：一个正的偏移量，它和序号段的值相加表示最后一个紧急数据的下一字节的序号。

原文链接：https://blog.csdn.net/ythunder/java/article/details/65664309



# TCP 三次握手

![image-20200613172314931](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613172314931.png)

第一次握手：（A>B）

SYN=1,seq=x

第二握握手：(B->A)

SYN=1,seq=y,ACK=1,ack=x+1

第三次握手：（A->B）

ACK=1,ack=y+1，seq=x+1



专业回答

![image-20200613191632376](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613191632376.png)



## 总结

总的来说，客户端要确保服务端是否能够收到客户端发送的消息，服务端要确保客户端是否能够收到服务端发送的消息



例子：（不专业，不建议）

A:请问我什么时候来面试方便呢

B:大概明天9点可以；(证明A发送消息成功，B也成功收到消息）（此时B不知道A是否收到消息）

A:好的；（证明A成功收到消息，B也成功发送了消息）



# 小记

**source port & destination prot:**

进程间通信，可由管道，内存共享，信号量，消息队列等方法进行通信，进程间通信的最基本前提是，唯一的标识一个进程，通过这个唯一标识找到这个进程，在本地通信中可以使用pid来唯一标识一个进程，但是pid只在本地唯一，如果两个进程在不通的两台计算机，要进行通信的话，pid就不够用，这样就需要另外一个手段了，解决这个问题的方法就是在传输层中使用协议端口号，ip能够唯一标识主机，而tcp协议及端口号能唯一标识主机中的一个进程，这样我们可以利用ip地址+协议+端口号作为唯一标识去标识网络中的一个进程，也称为套接字  ，即socket

**suquence number(seq)**:

tcp传输的每个字节都是按顺序编号的



# 抓包软件

wireshark 



# 四次挥手

图解：

![image-20200613192520472](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613192520472.png)



解释：

![image-20200613192401882](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613192401882.png)



服务器出现大量close_waite状态

![image-20200613194844751](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613194844751.png)

 # 查看服务器close_wait 数量的方式

![image-20200613195023669](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613195023669.png)

# UDP特点

![](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613195244662.png)



# TCP 与UDP 的区别

![image-20200613195527522](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200613195527522.png)