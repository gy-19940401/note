# Java 网络编程

## 1、Socket

主机之间通过 （TCP/UDP）建立连接，实现通信的过程。

### 1、短链接与长连接

~~~
http 是 无状态的协议。
TCP
	建立连接 ： 三次握手
	关闭连接 ： 四次挥手

1、短链接。
	连接 - 传输数据 - 关闭连接
	
2、长连接。（操作频繁，点对点的）
	建立连接 - 传输数据 - 保持连接 -... - 关闭连接
~~~

### 2、网络通讯的流程

~~~
1、建立连接

2、读取网络数据

3、回写数据
~~~

![image-20220729210259642](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220729210259642.png)

## 2、RPC 框架

remote procedure call 

![image-20220729211415217](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220729211415217.png)

~~~
1、高效的序列化方式
	seriable
	fastjson
2、服务的注册与发现
	对内：Dubbo ：TCP层面 （吞吐量 = 2 * http）
	对外：Cloud ：http(应用层)，更通用，Restful，生态更加完善
3、负载均衡
~~~

![image-20220729213021614](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220729213021614.png)