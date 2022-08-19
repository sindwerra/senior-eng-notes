### Django ORM类的三种继承方式
1. 抽象继承（不产生实际表）
~~~Python
class ModelName:
	... 
	class Meta:
		abstract = True
~~~
2. 实际继承

3. 代理继承
只在Python层级父子类有不同表现，数据库表为同一张
~~~Python
class ParentName:
	...

class ChildName:
	...
	class Meta:
		proxy = True
		
	def custom_method():
		...
		
child_name_object.custom_method()
~~~

### Django的日志组件层级
Logger -> Filter -> Handler -> Formatter

### Django的日志层级
Debug, Info, Warning, Error, Critical

#### ATOMIC_REQUESTS配置
单个视图函数的数据库操作全部会被包装在一个事务中