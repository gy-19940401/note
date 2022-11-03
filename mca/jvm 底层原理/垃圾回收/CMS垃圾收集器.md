# CMS垃圾收集器

最短停顿时间

## 1、CMS 初始标记为什么不使用多线程。

~~~
在 JDK1.7时，初始标记是单线程
   JDK1.8后，默认是并行；-XX:+CMSParaLLellnitialMarkEnable
初始标记：标记根对象
~~~

## 2.两种模式和一种策略

![](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220704221051855.png)

~~~
1、当老年代被回收时，怎么判断 一个对象是否存活
	1.扫描新生代，来确定一个对象是否存活。
2、需要扫描新生代，全量扫描慢
	会慢，为了解决这个问题，我们先减少新生代的存活对象。
	CMCSchduleRemarkEdenSizeThreashould 2M
	CMSSchduleRemarkEdenPenetration 50%
	当 eden 区使用空间超过 2M 时，启动可中断的并发预清理，到 eden 可用空间达 50% 时（中断）但不结束，进入 remark 重新标记阶段。
	+CMSScavengBeforeRemark remark之前强制进行一次 minorGC
	
	为什么要 可中断的并发标记
~~~

~~~
常见概念
	1、记忆集：非收集区指向收集区的指针集合。（跨代引用）-----》》》》卡表是实现（字节数组）
	卡表（记忆集的实现）：字符数组，将内存分为 2^9字节大小的块（卡页），用一个字节来标识这个块。当某一块引用发生变化，就将卡表对应索引致为1
~~~



### 1、常规模式

正常情况下的全流程的垃圾收集

### 2、Foregroud CMS

并发失败的模式

~~~
并发标记过程中分配对象，并且空间不足时，直接 标记清除阶段。
	1、回收不及时
	2、对象晋升（空间不连续）
	-XX:CMSInitiaingOccupancyOnFraction（70%） 老年代占比达到一定程度就 FullGC
	-XX:+UseCMSInitiainggOccupancyOnly
	 假如不设置以上，则 默认 92%
~~~

![image-20220705204704751](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220705204704751.png)

~~~
	MSC(标记清除压缩 算法；不推荐的一种模式)
	-XX:UseomCMSCompactAtFullCollection
	-XX:CMSFullGCsBeforeCompaction=0
多少次 fullGC 后，压缩堆内存，0表示 每次都压缩
~~~

### 3、三色标记

~~~
	黑色：已经被收集器访问过，所有引用均被访问过
	灰色：对象被收集器访问过，但至少有一个引用没被对象访问过。
	白色：没有被收集器访问过的对象。
	
	存在问题：多标（当成不是垃圾）：该回收的没被回收。浮动垃圾
			漏标：不该回收的被回收。
			
	1.并发标记时被标黑后，本该回收没回收。浮动垃圾
	2.灰色对象断开白色对象，黑色重新引用白色对象。
		增量更新：卡表记录脏卡，重新标记
		原始快照：增加引用的时候，记录这个引用
~~~

### 4、CMS常用参数

![image-20220705212202693](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220705212202693.png)

![image-20220705212337904](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220705212337904.png)

![image-20220705212434493](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220705212434493.png)

### 5、CMS线程数

![image-20220705212602939](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220705212602939.png)

![image-20220705212655001](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220705212655001.png)