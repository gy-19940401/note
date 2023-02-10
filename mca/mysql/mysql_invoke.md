# 0.sql 的执行流程

~~~
一条 SQL 语句的执行流程，需要经过很多的步骤
~~~

## 1、执行的总体流程

![sql执行总体流程](../typroImage/sql_invoke_primary.png)

### 1.1 涉及到的组件

#### a、建立连接

~~~text
    Client 与 Server 建立连接、进行鉴权、保持连接、管理连接。
        最大连接数 : SHOW VARIABLES LIKE 'max_connections'; (default : 151) 
        当前连接数 : SHOW PROCESSLIST
~~~

#### b、查询缓存

~~~text
    数据查询的结果会保存到一个引用表中，通过一个哈希值引用，这个哈希值包括了以下因素，即查询本身、当前要查询的数据库、客户端协议的版本等一些其他可能影响返回结果的信息
    
    缺点 : 将连接信息 + sql 脚本 等原始信息当作查询缓存的条件，任何修改都可能导致 缓存不命中
          当查询条件中存在不确定的 数据 或 函数 时，缓存也可能不会命中（NOW()，CURRENT_DATE()）
          
    不建议使用 而且在 8 以上缓存机制已经被 移除
~~~

#### c、分析器

##### c.1、词法分析

~~~
    从左到右一个字符、一个字符地输入，然后根据构词规则识别单词。
~~~

![word_analyze](../typroImage/sql_invoke_word_analyze.png)

##### c.2、语法分析

~~~
    语法解析，判断输入的这个 SQL 语句是否满足 MySQL 语法.
~~~

![language_analyze](../typroImage/sql_invoke_language_analyze.png)

##### c.3、语义分析

~~~
    对语句中涉及的 表、索引、视图 等对象进行解析，并对照数据字典检查这些对象的名称以及相关结构
~~~

#### d、预处理器

~~~
即时 SQL : 一次编译，单次运行
预处理 SQL : 一次编译 多次执行
            每次执行的时候只有个别的值不同（比如 select 的 where 子句值不同，update 的 set 子句值不同，insert 的 values 值不同）
~~~

#### e、优化器

~~~
按照一定原则来计算目标SQL在当前情形下最有效的执行路径,优化器的目的是为了得到目标SQL的执行计划。
分为两种方式
    RBO : Rule_Based
    CBO : Cost_Based
~~~

##### e.1 RBO

~~~text
    一组内置的规则，这些规则是硬编码在数据库的编码中 --- 规则恒定
        其中之一 : 有索引使用索引。那么所有带有索引的表在任何情况下都会走索引
~~~

##### e.2 CBO

~~~text
    从目标诸多的执行路径中选择一个成本最小的执行路径来作为执行计划
    综合 相关统计信息计算出的 对应的步骤的IO，CPU等消耗 --- 计算复杂
~~~

[优化后获得 sql 执行计划](./mysql_invoke_plan.md)

#### f、执行器（查询存储引擎）

~~~text
    开始执行的时候，首先要确认我们是否有操作这个表的权限，如果没有权限则会返回没有权限的错误
    如果有权限，就打开表权限执行，打开表的时候执行器会根据表的引擎定义，去使用这个引擎提供的接口。
~~~

![invoke](../typroImage/sql_invoke_engine.png)

##### f.1. innodb_db 三大特性

###### f.1.1、自适应hash

类似于缓存

~~~
内部监控索引表，某个索引经常使用（热数据），在内部创建一个 hash 索引，下次查询时，根据 hash 索引查找

show engine innodb status

SHOW VARIABLES LIKE "innodb_adaptive%"
~~~

###### f.1.2、buffer Pool

缓存磁盘页（16K 的数据）

~~~
SHOW VARIABLES LIKE "innodb_buffer%" : 8M 
~~~

###### f.1.3、双写机制（DoubleWrite buffer）刷盘

[数据页的可靠性 -- 双写机制链接](https://zhuanlan.zhihu.com/p/272720373#:~:text=Double%20write,buffer%20%E5%AE%83%E6%98%AF%E5%9C%A8%E7%89%A9%E7%90%86%E6%96%87%E4%BB%B6%E4%B8%8A%E7%9A%84%E4%B8%80%E4%B8%AAbuffer%2C%20%E5%85%B6%E5%AE%9E%E4%B9%9F%E5%B0%B1%E6%98%AFfile%EF%BC%8C%E6%89%80%E4%BB%A5%E5%AE%83%E4%BC%9A%E5%AF%BC%E8%87%B4%E7%B3%BB%E7%BB%9F%E6%9C%89%E6%9B%B4%E5%A4%9A%E7%9A%84fsync%E6%93%8D%E4%BD%9C%EF%BC%8C%E8%80%8C%E5%9B%A0%E4%B8%BA%E7%A1%AC%E7%9B%98%E7%9A%84fsync%E6%80%A7%E8%83%BD%E9%97%AE%E9%A2%98%EF%BC%8C%E6%89%80%E4%BB%A5%E4%B9%9F%E4%BC%9A%E5%BD%B1%E5%93%8D%E5%88%B0%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E6%95%B4%E4%BD%93%E6%80%A7%E8%83%BD%E3%80%82)


数据库崩溃恢复时保证数据不丢失

~~~text
    数据刷脏的时候，先拷贝到 内存 doublewrite buffer 中
    然后再 顺序写到 磁盘 共享表空间中（doublewrite buffer）
    然后再 离散写到 数据文件中
~~~