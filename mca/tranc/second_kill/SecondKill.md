# 0、[实现高并发秒杀的七种方式](https://mp.weixin.qq.com/s/PVMlEKvmeFoYI2gCQs6rFA)

## 1、进入业务层之前加锁

~~~
在 controller 层加锁，保证业务代码执行完成后才会释放锁

存在问题 ： 因为锁的原因可能会拒绝掉部分有效请求
          加锁方式不够优雅
~~~

## 2、基于 AOP 实现加锁

~~~
基于 aop around 环绕方法实现 业务逻辑加锁 
~~~

## 3、基于数据库 行锁的 悲观锁 （for update）

~~~
查询商品余额时，使用 for updata 加 行锁，保证在事务提交后才会释放行锁
~~~

## 4、基于数据库 表锁的 悲观锁 （update）

~~~
直接扣减余额，基于 update 操作的表锁 , 注意事务提交的时机
~~~

## 5、基于数据库 version 乐观锁 （version）

~~~
直接扣减余额，基于 version 操作的乐观锁

存在问题 ： 会 拒绝大量有效请求；；一般不适用
~~~

## 6、基于 阻塞队列（消息队列） 异步实现  -- ApplicationRunner（扩展接口                 ）

~~~
消息队列缓存用户请求，异步线程处理对应请求
~~~

## 7、基于 Disruptor高性能队列 异步实现

~~~
消息队列缓存用户请求，异步线程处理对应请求
~~~