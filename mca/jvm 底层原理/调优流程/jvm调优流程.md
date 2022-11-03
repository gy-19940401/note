# 1、JVM调优流程

## 1、常用命令

### 1、查看 java 进程（jps）

### 2、查看 某个 进程状态 (JINFO)

~~~
jinfo
1、查看某个 jvm 参数
	jinfo -flag jvmParam pid
2、查看 修改过的 jvm 参数
	jinfo -flags pid
~~~

### 3、查看进程性能情况 （jstat）

~~~
1、查看进程 类加载 信息
	jstat -class pid 查询时间间隔 查询次数
2、查看进程 GC 信息
	jstat -GC pid 查询时间间隔 查询次数
~~~

### 4、查看当前线程信息 （jstack）

~~~
1、查看线程信息
	jstack pid 
~~~

### 5、查看jvm 内存信息(jmap)

~~~
1、查看堆内存信息快照
	jmap -heap pid
	
	增加 -XX:HeapDumpOnOutOfMemoryError 
		-XX:HeapDumpPath=heap.hprof 导出 OOM dump文件
~~~

# 2、jvm优化逻辑	

### 1、代码结构的优化

#### 1、编译时优化

~~~
1、解释执行模式： java -Xint 
2、混合执行模式：（默认）java -Xmixed
3、编译执行模式： java -Xcomp
~~~



![image-20220723202437627](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220723202437627.png)

~~~
1、初始时
	执行代码的同时，一边解释编译，一边执行代码。
	缺点：热点代码{
		1、主要构成
			a、循环
			b、频繁调用的方法
		2、判断方式
			a、基于采样的热点检测
				1、
			b、基于计数器的热点检测
				优点：结果准确。
				缺点：实现复杂。
					解决：1.方法调用计数器。
						 2、被多次执行的循环体。-XXOnStackReplacePercentage client=14000;server 1500
		
	}的解释，非常消耗时间。
2、即时编译器 （C1，C2）分层编译 （-XX:+TieredCompilation 开启 分层编译；；关闭分层，默认C2）
	0、解释执行
	1、简单的 C1 编译：不开启 Profiling{ jvm 性能监控 }
	2、受限的 C1 编译代码：执行 方法调用次数，循环次数 Profiling C1 编译
	3、完全 C1 编译代码：我们 Profiling 里面所有的代码，会被 C1 执行
	4、C2 编译代码：优化的级别
	
	注：编译后的代码存放在方法区{
		1、类信息
		2、静态变量
		3、常量
		4、即时编译的结果（Code cache{
			JNI,...
			1、InitialCodeCacheSize 初始大小 160K
			2、ReservedCodeCacheSize 最大值 48M
			2、CodeCacheExpansionSize 代码缓存扩展大小 32k 或者 64k
			
			-XX:+UseCodeCache 能够回收 缓存空间
			
			},）
	}
~~~

![image-20220723204015750](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220723204015750.png)

#### 2、运行时优化

~~~
方法内联：避免热点方法的调用性能的开销。（栈帧的优化）
	1、热点代码。（client 15000, server 10000）
	2、方法体本身不大。（小于325字节。非热点代码 35 字节以下）
	3、避免多态，尽量使用 private,static,final 修饰
~~~

#### 3、逃逸分析

方法级的分析

-XX:+DoEscapeAnalysis （开启逃逸分析，JDK8 默认开启）

##### 1、什么是逃逸现象

~~~
本质是：对象指针的逃逸。

变量（对象）的 指针 被 方法 返回后，超出 对象本身的作用域。
	1、假如对象不会逃逸，就可以将对象 直接分配至 方法的栈帧上，避免分配到堆中，减少 gc
~~~

##### 2、基于对象逃逸的优化

~~~
	1、栈上分配：将对象分配到 当前线程栈上。
		存在问题：大的对象怎么存？（标量替换）
	2、同步消除：
		1、同步锁消除：同一线程，能够自动消除锁的存在。
	3、标量替换：将对象的成员变量替换为 一个一个的标量。
		标量：不可以再细分的量 基础数据类型
		聚合量：
~~~

##### 3、触发逃逸分析的条件

尝试老年代分配 （悲观策略）

~~~
逃逸分析只发生在 JIT 即时编译中
	1、对象被赋值给堆中 对象的字段 或者 静态变量。
	2、对象被 传值到 不确定的 代码中运行。 
	
	全局逃逸，局部逃逸，没有逃逸。

悲观策略
	1、大对象分配
	2、
	3、
~~~



![image-20220724222128518](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220724222128518.png)

##### 4、TLAB(线程本地分配缓冲区)

~~~
堆内存分配的时候，由于多线程的存在，分配对象会 存在线程安全问题。

为了解决这个问题，为每一个线程 在 堆中 分配一个独占的 空间。
~~~

![image-20220724223741585](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220724223741585.png)

### 2、jvm 优化

#### 1、接错

##### 1、OOM；

~~~
1、内存分配不合理。
	1、堆分配（50%-75%）
	2、

限流，降级，tomcat,mysql,服务器，微服务，jvm
~~~

![image-20220724230131645](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20220724230131645.png)

##### 2、GC 频繁；

##### 3、CPU 突然过高