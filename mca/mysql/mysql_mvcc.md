#MVCC多版本并发控制

~~~
读不加锁，写乐观锁
快照读
~~~

## 1、实现原理

~~~
1、记录的隐性字段（多个）
	trx_id --> 事务ID 对于比当前事务ID 大的事务是不可见的。
	版本链 --> 不同事务 || 相同事务 对 同一记录进行修改的时候，导致该记录的 undo 成为一条（记录）链表；修改前后的值
	roll_ptr --> 指向 undo 日志的指针
2、undo 日志(原子性)
3、Read View
	m_ids:生成 Read view 时，当前活跃的事务ID
	min_trx_id : Min(m_ids)
	max_trx_id : max(m_ids) + 1
	creater_trx_id:创建 Read View 时，当前事务ID
	
	访问规则：
		1、当前事务ID == creater_trx_id --> 同一事务产生数据，当前事务可见
		2、当前事务ID < min_trx_id -->能够访问数据
		3、当前事务ID > max_trx_id -->不能访问数据
		4、当前事务ID in m_ids ? 不能访问数据 : 能够访问数据（事务（非活跃）已经提交）
		5、某个版本 对 当前事务不可见，顺着 版本链往前找（直到找不到）第一个 不在 m_ids 中的事务
		
两种事务隔离，怎么实现
	1、READ COMMIT
		
	2、REPEATABLE
~~~