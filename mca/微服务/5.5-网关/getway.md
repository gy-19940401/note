# 1、三大核心

![image-20221125220121588](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221125220121588.png)

## 1、路由 || 动态路由

#### 1、路由规则

~~~
请求：/order/** 
~~~



## 2、断言

~~~
对 请求头 || 请求体 进行断言校验
~~~



## 3、过滤器链

~~~
1、DefaultFilter
2、当前路由Filter
3、GlobalFilter

通过 @Order 指定排序优先级（value 越小，优先级越高），当配置一样时 （1，2，3）的顺序
~~~



![image-20221125215141694](C:\Users\CSB7D0\Desktop\mca\typroImage\image-20221125215141694.png)