# 1、支持的版本

~~~
1、grovy ： 性能差
2、painless : 类似java
	1、性能 比 DSL 差
	2、不适用于简单场景
3、expression 查询简单数据类型字段
~~~

# 2、painLess

## 0、基本语法

~~~
POST index/_update/id
{
	"script":{
		"source":"ctx._source.<fieldName>",// ctx 上下文；
		"params":{
			
		}
	}
}

1、ctx._source.<fieldName>.add() 给数据新增（追加写）
2、ctx.op="delete" 删除数据
3、upsert : 数据存在 执行 script 修改；否则执行 upsert 插入
~~~

## 1、创建查询模板

~~~
POST _script/name
{
	"script":{
		"source":{
			
		}
	}
}

使用
GET index/_search
{
	"name":{
		"script":{
			"script_name":"",
			"params":{
				 
			}
		}
	}
}
~~~

## 2、函数式编程

~~~
POST index/_update/id
{
	"script":{
		"source":"""
			写代码块
		""",// ctx 上下文；
		"params":{
			
		}
	}
}
~~~

