# ZGC

~~~
1.10ms 以内的停顿时间
2.支持 TB 级别的堆内存（8M-16T）
3.停顿时间与堆大小无关

初始标记
并发标记
重新标记
并发转移准备
初始转移
并发转移
~~~

![image-20220719231141562](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220719231141562.png)

## 三大特征

### 1、多重映射

~~~
 物理内存 映射成 虚拟内存
 			   三个视图：
 					mark0
 					mark1
 					remark
~~~

### 2、读屏障（对象转移的探查）

~~~
	G1：写屏障
	
	探查对象状态
	
	从堆内存 读取 对象引用，才需要加入 读屏障
	Object o = obj.FieldA
~~~

### 3、指针染色

~~~
	通过对象指针 记录 对象的信息
	
	内存指针 64 位机器 高 18 位 不支持寻址 64G
	
	JDK15 之前 最大支持 4G(42 bit)
	JDK15 之后 最大支持 16G(44 bit)
	取出 4 bit 支持指针染色
	 4             3          2        1
	 FInalizable   Remapped   Marked1  marked0
	
~~~

## 2、并发处理算法

初始化阶段   ：remapped 视图

标记阶段       ：M0/M1 视图

转移阶段	   ：Remapped 视图



### 1、标记阶段（remapped 转到 m0/m1）

~~~

~~~

### 2、转移阶段（m0/m1 切换到 remapped）

~~~
 
~~~

### 3、调优参数

~~~
-Xms -Xmx 16G
-XX:+UseZGC
-XX:ConcGCThread：默认 总核数/8

~~~

### 4、垃圾回收触发时机

~~~
1、阻塞内存分配请求：垃圾来不及回收，导致部分线程阻塞；（Allocation Stall）并发失败
2、基于分配速率的自适应算法：最主要的 GC 触发方式。根据内存分配以及GC时间，计算内存到达什么阈值时触发 GC；ZAllocationSpikeTolerance 设置，默认是2，值越大越早触发 GC(Allocation Rate)
3、基于固定时间间隔 ：隔段时间触发 Gc
~~~

## 3、常用命令

### 1、jps

### 2、jinfo

### 3、jstat

### 4、jstack

### 5、jinfo