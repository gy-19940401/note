## 1、feign || openFeign

~~~
基于 动态代理机制
~~~

![image-20221125220747148](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221125220747148.png)

### 1、核心

~~~
1、@EnableFeignClients
2、@FeignClient --> 通过动态代理调用 @RequestMapping 上的URL --> http 的 restTemplete
~~~

