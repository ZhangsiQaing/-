## 单个Pair RDD转化操作
函数名                   目的
reduceByKey(fuc)        合并具有相同键的值
groupByKey()            对具有相同键的值进行分组
combineByKey()   	    使用不同的返回类型合并具有相同键的值
mapValues(func)         对 pair RDD 中的每个值应用一个函数而不改变键
flatMapValues(func)	    对 pair RDD 中的每个值应用一个返回迭代器的函数，然后对返回的每个元素都生成一个对应原键的键值对记录。
keys()	                返回一个仅包含键的 RDD
values()	            返回一个仅包含值的 RDD
sortByKey()	            返回一个根据键排序的 RDD

## 两个RDD转化操作
函数名	                      目的
subtractByKey(other)	    删掉 RDD 中键与 other RDD 中的键相同的元素
join(other)	                对两个 RDD 进行内连接
rightOuterJoin(other)	    对两个 RDD 进行连接操作，确保第一个 RDD 的键必须存在（右外连接）
leftOuterJoin(other)	    对两个 RDD 进行连接操作，确保第二个 RDD 的键必须存在（左外连接）
cogroup(other)	            将两个 RDD 中拥有相同键的数据分组到一起


## Pair RDD行动操作
函数	                描述
countByKey()	      对每个键对应的元素分别计数
collectAsMap()	      将结果以映射表的形式返回，以便查询
lookup(key)	          返回给定键对应的所有值
