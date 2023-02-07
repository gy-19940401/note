### 消息数据结构

~~~
1、commit
2、flush
~~~

## 1、commitLog

~~~
存放全局消息数据
~~~



## 2、两种数据同步结构

### 1、不使用对外内存 (无commit，只需 flush)

![image-20230110211553111](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20230110211553111.png)

### 2、使用堆外内存（commit : 堆外内存 -> mappedByteBuffer；+ flush）

![image-20230110211634540](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20230110211634540.png)