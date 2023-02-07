## 1、dispath

~~~
将消息，分发给对应的 comsumeQueue
30万 * 20{
		commitLog index : 8字节
		msg size        : 4字节
		tags hash       : 8字节
		}
~~~

### 1、CommitLogDispatcherBuilderConsumeQueue

~~~
1、根据 topic + queueId 获取对应的 ConsumeQueue
2、
~~~

### 2、CommitLogDispatcherBuilderIndex

## 2、IndexFile

![image-20230110215026465](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20230110215026465.png)



~~~
hash 冲突时（Next index offset），头插数据已解决 hash 冲突
~~~



![image-20230110215435210](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20230110215435210.png)

## 3、consumerQueue

## 4、拉取数据的流程

本质都是 pull

~~~
push
	dufaultMqConsumerPush
pull


调优 
	循环 pull 消息，性能消耗解决
~~~

~~~
pullMessage -- 拉取消息的请求参数 -- processRequest

broker -- 路由数据 （Queue,filter,topic,QueueId）-- ConsumeQueue （20字节 = offset + size + tags hash）
	未获取到数据时，再次封装一个阻塞请求，尝试获取消息，未获得时再返回 null {
		为 null 时，不会执行消息处理逻辑（pullCallback） -- pullRequestHolderServer -- processRequest
		
		不为 null [ -- executeRequestWhenWakeup] -- channel.writeAndFlush()
		a、避免重复请求时的逻辑
		b、正常返回，拉取到数据 -- 正常流程
		
	}
	
	{
		集群消费模式，维护组内的一个消费索引；双层 ConcurrentHashMap -- 持久化到硬盘
	}
	
pullCallback -- 
	1、拉取到的消息处理 -- ConsumerMessageService -> 触发 -> MessageListener [orderly || Concurrent]
    2、再次触发消息拉取请求（？？？？）
    

~~~

## 特殊队列

### 1、重试队列 (sendMessageBack)

~~~
code : consumer_send_message_back

重试时 ： 间隔时间需要递增 --- 基于 延迟队列 实现

消息如何拉取 ： {
	集群模式 -- 消费正常消息时，会同时拉取 重试队列
}
~~~



### 2、死信队列（会覆盖重试）

~~~
重试次数达到一定的限制 {
	可以配置 ： maxReconsumeTimes
	|| delayLevel < 0	
}

~~~



### 3、延迟队列 （对外 API）

~~~
topic : SCHEDULE_TOPIC_延迟级别 -- 放回加入时的队列

server : ScheduleMessageService {
	1、每个延迟队列一个 task
	2、当队列能获取数据时，就获取消息的 realTopic 并推送
}
~~~



### 4、事务队列

### 5、操作队列