# 1、 五大核心组件

## 1、注册中心

### 1、nacos(AP,CP)

![image-20221024201923962](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221024201923962.png)

~~~
1、服务注册 --> Map<serviceName,addr>
2、健康检测 --> 心跳检测 ： 每隔 15s 检测一次；连续两次不健康 当前服务以下线
3、服务剔除 --> 剔除 被认为已经下线的服务
4、服务发现 --> 每个服务从注册中心获取健康状态的全部服务信息
5、负载均衡 --> 存在多个节点时决定调用那个服务；(轮询；hash...)；ribbon || feign ;; nacos 客户端 集成 ribbon
6、服务同步 --> gRPC（长连接）{
	1.X --> 心跳多，无效查询多，感知变化慢，连接消耗大，资源空耗严重
	2.X --> 发布订阅模型，对于下线的服务能够立马通知服务端 ？？？事件流程
}

~~~

#### 1、注册中心

~~~

~~~



## 2、负载均衡

### 1、DNS负载均衡 （CDN）

~~~
DNS 分发 : 根据 CDN 对 相同 域名得 服务 就进 发起请求
~~~



## 3、断路器

## 4、分布式配置中心

### 1、nacos

~~~
命名空间 
DataID
分组

支持动态配置
~~~



## 5、网关

# 2、三大配件

## 1、链路追踪

# 3、二大中间件

## 1、消息队列

### 1、rabbitMQ

#### 0、五大工作模式

##### 1、简单模式

![](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220908220701466.png)

##### 2、work

![](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220908220713926.png)

##### 3、发布订阅

![](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220908220731749.png)

##### 4、router

![](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220908220745566.png)

##### 5、topic

![](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220908220802767.png)

#### 1、自动应答

#### 2、重要队列

##### 1、死信队列

##### 2、延时队列

### 2、rocketMQ

## 2、缓存（NoSql）

### 1、redis

~~~
1、基本类型
	1、string
	2、list
	3、hash
	4、set
	5、zset
2、底层数据结构(算法 临界值)
	string : sds
	list : zplist quickList
	hash : hash dict
	set  : intset hash
	zset : (长整型512) (个数128长度64字节) skipList
3、持久化
	AOF ： 增量 操作记录
	RDB ： 二进制日志 
4、淘汰策略
	1、LRU : 时效性问题 某一时刻 很多数据写到缓存 存频率
	2、LFU : 16(时间) + 8(频率)
5、过期策略
	1、主动过期
		1、查询时判断是否设置过期时间
	2、被动过期
		1、设置过期时间 hash桶（数据结构）20个 
6、分布式锁
7、穿透 击穿 雪崩
8、哨兵 主从 集群
~~~



# 4、三大分布式解决方案

## 1、分布式锁

### 1、基于数据库

### 2、基于 redis

### 3、基于 ZK

## 2、分布式事务

### 1、seata

## 3、分布式定时任务

~~~
1、触发器 ：Trigger
2、调度器 ：Scheduler
3、描述器 ：JobDetail
~~~

### 1、Quartz

### 2、Elastic-job

### 3、XXL-job

![image-20221019222426974](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221019222426974.png)

#### 1、数据分片任务

~~~
1、为什么要分片
	多个任务的情况下，多个执行器同时被调度，但是 处理的数据不同

2、如何实现分片
~~~

![image-20221020221819935](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221020221819935.png)