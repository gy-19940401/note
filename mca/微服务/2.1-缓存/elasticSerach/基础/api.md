## 1、查询节点健康状态

~~~
单节点：_cat/health
集 群： _cluster/health
~~~

## 2、REST API

### 1、CRUD

#### 1、创建索引

~~~
put index[?pertty]
	pretty : 格式化执行结果输出
~~~

#### 2、查询索引

~~~
GET _cat/index[?V] -- 获取索引状态信息
	[?V] : 给出 title
GET /index/_search -- 获取索引信息
~~~

#### 修改索引

~~~json
POST /nft/_mapping
{
	"properties": {
		"field":{
			"fielddata":"true/false":[做聚合/不做聚合]
		}
	}
}
~~~



#### 3、删除索引

~~~
delete /index[?pertty]
~~~

#### 4、插入数据

~~~
PUT /index/_doc/id jSONDATA
~~~

#### 5、修改数据

~~~
1、PUT /index/_doc/id JSONDATA 覆盖相同id，全量替换
2、POST /index/_doc/id/_update -- 不推荐
{
	"doc":{
		"filed":value
	}
}

3、POST /index/_update/id
{
	"doc":{
		"filed":value
	}
}
~~~

#### 6、查询数据

~~~
GET /index/_doc/id 查询指定ID的数据
~~~

#### 7、删除数据

~~~
DELETE /index/_doc/id 删除指定id 的数据
~~~

### 2、分词器

~~~
_analyze
~~~

