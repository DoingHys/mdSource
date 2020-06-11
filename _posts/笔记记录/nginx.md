# location 匹配优先级

= 进行普通字符精确匹配，也就是完全匹配

^~ 表示普通字符匹配，使用前缀匹配

~ \ ~ * 表示执行一个正则匹配



# try_files的使用

按顺序检测文件是否存在,存在则访问，不存在则访问下一个

```
location /{
	try_files $uri $uri/ //index.php;
}
```



# Nginx的alias和root的区别

**root:**

![image-20200428162711647](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200428162711647.png)

**alias:**

![image-20200428162756937](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200428162756937.png)

# 如何传递用户真实ip地址

在一级代理设置头

set x_real_ip=$remote_addr 

后端服务

$x_real_ip =IP1



# 其他错误

上传文件太大会报:  413 Request Entity Too Large

后台服务没响应： 502 bad gateway

后端服务执行超时: 504 gateway time-out（默认X秒）



# niginx +lua 防火墙

![image-20200428170342382](C:\Users\vinti\AppData\Roaming\Typora\typora-user-images\image-20200428170342382.png)

nginx 设置请求访问频率达到，控制cc攻击





# nginx官网

http://nginx.org/