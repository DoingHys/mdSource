# get请求和post请求的区别

Http报文层面：get请求请求信息放在url，post请求信息放在报文体种

数据库层面：get请求符合幂等性和安全性，post不符合

其它层面：get可被缓存、被存储，而post不行



# cookie和session的区别

**cookie：**

![image-20200614010219172](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200614010219172.png)

![image-20200614010332225](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200614010332225.png)



session:

![image-20200614011012436](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200614011012436.png)



sesiion的实现方式：

1. 通过使用cookie来实现：生成给每个jsession生成唯一的sessionid，并通过cookie发送给客户端，当客户端发起新的请求的时候带上JSESSIONID,服务器就能找到对应的session
2. 使用url回写来实现，把服务器传过来的session放在url中，然后每次请求服务器，都能通过url获取session



cookie和session的区别：

 cookie数据存放在客户的浏览器上，session存放在服务器上

session相对于cookie更安全

若考虑轻服务负担， 应当使用cookie



# HTTP 和 HTTPS 的区别

**SSL:**安全套接层

* 为网络通信提供安全及数据完整性的一种安全协议
* 是操作系统对外的API,SSL3.0后更名为TLS
* 采用身份验正和数据加密保证网络通信的安全性和数据完整性

**加密：**

![image-20200614020846249](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200614020846249.png)

![image-20200614021424350](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200614021424350.png)



**HTTP和HTTPS的区别**

![image-20200614021804660](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200614021804660.png)