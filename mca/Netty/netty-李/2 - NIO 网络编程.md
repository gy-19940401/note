# IO 模型

~~~
阻塞 与 非阻塞：要不要死等。数据没来就一直在原地等着，什么都不做。
异步 与 同步：结果谁来获取。同步，调用方去查询。非同步，处理方发送通知。
~~~



## 1、BIO（阻塞）

## 2、NIO（非阻塞）

### 1、三大组件

~~~
1、Selector
2、Channel
3、buffer缓冲区
~~~

![image-20220922203757465](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220922203757465.png)

### 2、实现流程

~~~
1、通过 Channel 与服务端建立连接。
2、事件处理
3、断开连接
~~~

### 3、Reactor

#### 1、单线程模式

![image-20220730211858685](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220730211858685.png)

#### 2、多线程模式

![image-20220730212020469](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220730212020469.png)

## 3、AIO（异步）