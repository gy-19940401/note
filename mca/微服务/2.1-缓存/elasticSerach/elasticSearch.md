~~~
plugin : elasticSearch head
~~~



# 1、倒排索引

## 1、索引

~~~
保存在磁盘中的文件，能够帮助提升加锁效率
~~~

| 正排索引 | 文档 --> 词项 | 当前文档包含了哪些词项   | 适合做统计逻辑 |
| -------- | ------------- | ------------------------ | -------------- |
| 倒排索引 | 词项 --> 文档 | 当前词项在那些文档出现过 | 适合做查找逻辑 |

![image-20221026213820960](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221026213820960.png)

## 2、倒排索引

~~~
分词的词项 与 id 的 映射关系

1、分词 ：对创建索引的字段进行分词 
	分词器
		词项字典 ：原始字段分词后的词项集合
		倒排表 ： 包含词项的 id ;(有序数组) 
		
2、规范化 ： 将相同含义的词进行简化（时态，语态，大小）
	减小词项的数量级
3、去重 ： 
	去掉重复的数据
4、字典序 ： 
~~~

## 3、FST

# 2、写入原理（写入换查询）

![image-20221026214326006](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221026214326006.png)

## 1、索引的创建与合并（refresh）

~~~
1、在内存中开辟一块空间 （JVM 的 10%）用于存放写入请求数据
2、当 buffer 满了 之后 （或者 1s 生成一次）
	将 buffer 里面的 数据写入到 segment file 文件中(段 / 索引文件) 可以直接查询
	并 发送通知请求 到 OS Cache（系统缓存）
3、当 segment file 接收到 OS Cache 的确认消息后，sgment file 可以对提供查询服务

segment file 存在内存中
	当 segemnt file 很多时 ，会 触发 segment file 合并 merge 出一个 新的大的 segment file。 
	
segment file 写入 osCache 的时机
	？？？
~~~



## 2、刷盘（flush : osCache -> osDisk）

~~~
1、OS Cache 定时触发刷盘，fsync 刷新数据到 磁盘
	1、osCache 写满了
	2、时间间隔 30分钟

可能存在部分数据的丢失，但是可以通过 transLog 保证丢失的数据的恢复
~~~



## 3、事务日志（translog）（容灾）

# 3、查询与检索

## 1、查询

~~~
给定查询范围，能够获取到明确的结果。
~~~



## 2、检索

~~~
根据查询关键，能够查出相关性的结果

相关性
	相关性评分算法
		TF-IDF : 词频出现的正相关
		BM25 ： 相关度是收敛的（控制因子 default --> K1=1.2; max=k1+1） 
~~~



# 3、调优

## 1、通用手段

~~~
1、读写分离 ：
2、职责单一 ：
3、业务分离 ： 
4、通用最小化 ：
~~~



## 2、集群架构

~~~
1、
2、三节点集群
3、
~~~



## 3、角色分配

~~~
1、master : 具备候选资格的角色
	1、管理当前集群的全部节点
	3、master 节点宕机后，需要重新选择出 master
	2、不存放数据
2、data : 数据节点

脑裂问题：
	原因 ：master 选举过程中，原本的一个集群由于网络的原因，导致集群被独立成两个集群
	解决 ：过半机制
~~~



## 4、写入性能调优

## 5、查询调优

## 6、数据结构

## 7、冷热集群架构

## 8、Mapping

## 9、代码层面

# 4、es数据结构

# 5、Master选举

~~~
1、FaultDetect
	1、masterFaultDetect
	2、nodeFaultDetect

2、脑裂问题
~~~

![image-20221026233752213](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221026233752213.png)

# 6、深度分页

~~~
数据跨分片索引

max_result_window : 出与 对 jvm 的保护，防止查询数据过大，导致 jvm 压力过大。

1、尝试避免深度分页
~~~



# 7、深度广度优先

# 8、es集群，

## 1、角色分配

~~~
1、master
2、node
~~~



## 2、分片数量

## 3、节点规模

## 4、冷热集群

~~~
1、hot 节点
2、warn 节点
3、cold 节点

ILM(划分策略)
	1、对节点进行划分，对数据进行分层。
~~~

# 9、eg

## 1、es每天定时生成索引，但体量差异大，怎么优化

~~~
1、通过 tollover 定时对 索引进行划分。
2、对于索引得管理 由于时序性得 存在 采用 ILM 对数据进行分层
3、在根据 dataStream 对数据进行保存以及管理（后备索引）
~~~

