# Netty

## 1、基本概念

~~~
1、Channel -- sockrt
2、EventLoop -- 控制流，多线程处理，并发
3、ChanneiFuture -- 异步通知


事件：
	1、入站数据、相关状态的更改而触发的事件
		连接激活，连结失活；
		读取数据，用户事件，错误事件
	2、出站事件（未来会出发的动作）
		打开，关闭 远程节点连接
		数据写入 socket
~~~

![image-20220730215528731](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220730215528731.png)

## 2、Netty 运用

### 1、实现 http 

### 2、实现 UDP 请求

~~~
单播模式：消息发送给唯一的主机。
广播模式：消息发送到局域网内的主机。
组播模式：将 交互之间的主机进行分组。
~~~

### 3、实现 WebSocket

![image-20220730223547042](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220730223547042.png)

## 3、TCP半包/粘包

~~~
client ： 发送 ABC - DEF

server :
	粘包：ABCDEF
	拆包：ABCD - EF
	
原因：1、TCP 是流式的协议，消息无边界 
     2、socket buffer 达到一定条件才会发送一次数据包
解决：找出消息的边界
~~~

![image-20220919221707934](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220919221707934.png)

## 4、解码器

### 1、一次解码器

解决 ： 粘包 半包问题

![image-20220920213409229](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220920213409229.png)

### 2、二次解码器

![image-20220920213549127](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220920213549127.png)

## 5、零拷贝

![image-20220920220310657](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220920220310657.png)

~~~
1、CPU 拷贝
	需要 CPU 发起，参与的 IO 过程
2、DMA 拷贝
	direct Memory Access . DMA控制器
~~~

### 1、sendFile

~~~
节约两次 cpu 拷贝的消耗

实现 FileChannel fileChannel= 流.getChannel()
     fileChainnel.transferTo(begin,size,socketChannel)
     
应用：
	kafaka
	redis
	MQ
~~~

## 6、锁优化技术

### 1、减少锁的粒度

~~~
BootStrap::init
~~~

### 2、减少锁对象空间占用

~~~
Atomic --- CAS
~~~

### 3、提高锁性能

~~~
LongAddr  <--> AtomicLong
~~~

![image-20220920225651998](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220920225651998.png)