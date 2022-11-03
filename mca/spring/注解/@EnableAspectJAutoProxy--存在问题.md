@EnableAspectJAutoProxy

~~~
开启 对 @Aspect 的支持

属性：
	1、proxyTargetClass {
		true  : 强制使用 cglib 代理;
		false : 默认使用 JDK 代理;
	}
	2、exposeProxy{
		true  : 通过 AOP 框架 将 暴露代理 对象 给 容器  --> aopContext.currentProxy 获得当前类的代理对象
		false : 默认值
	}
~~~

问题？

1、SpringBoot 在哪里自动转配的 这个的支持