# Conditional条件注解

参考：https://blog.csdn.net/JokerLJG/article/details/121099474

## 1、@conditional

~~~
通过本注解，可以配置条件判断；当条件都满足的时候，被注解的 目标才会被 Spring 容器处理

作用：
	1、控制 bean 是否需要被注册
	2、控制 config 是否需要被解析

使用：
	1、class
	2、method
	
参数：
	value : Class<? extend Condition> 默认一个 matches(); 返回true 代表 条件满足
~~~

## 2、扩展注解

使用时，需要保证被注解的 class 或者 method 是配置类

~~~
1、类上有@Component注解
2、类上有@Configuration注解
3、类上有@CompontentScan注解
4、类上有@Import注解
5、类上有@ImportResource注解
6、类中有@Bean标注的方法
~~~

| 注解类型                      | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| @ConditionalOnBean            | 当 <Strong>容器中 </Strong>  至少存在一个 指定 name 或 class 的Bean时，进行实例化 |
| @ConditionalOnMissingBean     | 当 <Strong>容器中 </Strong>      不存在       指定 name 或 class 的Bean都时，进行实例化 |
| @ConditionalOnClass           | 当 <Strong>当前类路径</Strong>  至少存在一个 指定 class 时，进行实例化 |
| @ConditionalOnMissingClass    | 当 <Strong>当前类路径</Strong> 指定class都不存在时，进行实例化 |
| @ConditionalOnSingleCandidate | 当 <Strong>容器中 </Strong>指定的Bean在只有一个，或者有多个但是指定了首选的Bean时触发实例化 |
| @ConditionalOnProperty        | 当 <Strong>指定的属性</Strong> 有指定的值时进行实例化        |
| @ConditionalOnResource        | 当 <Strong>类路径</Strong> 下有指定的资源时触发实例化        |
| @ConditionalOnJava            | 当 <Strong>JVM版本</Strong> 为指定的版本范围时触发实例化     |
| @ConditionalOnJndi            | 在 <Strong>JNDI存在</Strong> 的条件下触发实例化              |

