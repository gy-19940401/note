# 0、JAVA REST API

|              | JAVA REST API | JAVA  API（transport）                   |
| ------------ | ------------- | ---------------------------------------- |
| 传输数据格式 | json body     | binary format                            |
| 传输协议     | http          | tcp                                      |
| 存在问题     | 损失部分性能  | 重依赖（节点本身的基础支撑），版本兼容性 |

~~~
1、JVM 版本；（binary format）
~~~

![image-20221121213443492](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221121213443492.png)

# 1、Low Level client(链接 ES)

# 2、height Level client(基于 low)

# 3、sniffer

~~~
添加 sniffer 依赖
~~~

## 1、自动检测节点信息

~~~
Sniffer.builder(httpClient.setFailureListener(new SnifferFailureListerer()))
		.setSniffetIntervalNills() //多长时间嗅探一次 默认 5分钟
		.setSnifferFailureDelayMills() //嗅探失败后，间隔多长时间嗅探一次
		.setNodesSniffer
~~~

# 4、Spring Data

~~~
基于 spring 提供一致的数据访问层
~~~

