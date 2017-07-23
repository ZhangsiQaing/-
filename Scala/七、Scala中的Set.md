## Scala中的Set
* 无序不重复
* +/++/-/-- 都会创造一个新的Set
* +=/++=/-=/--= 不会创造一个新的Set

不可变Set
```
val set1 = Set(1,2,3,4,5)
val set2 = immutable.Set(7,8,9,10)
```

添加元素，实质上市产生一个新的set集合
```
val set3 = set1 + 200
```

判断是否包含元素
```
val set1 = Set(1,2,3,4,5)
println(s"是否包含0:${set1(0)}")
```

可变Set
```
import scala.collection.mutable
val set4 = mutable.Set[Int]()
```

添加元素
```
set4 += 12
set4 ++= Array(1,1,2,3,4,5)
set4(100) = true
```

删除
```
set4 -= 12
set4 --= Array(1,1,2,2,3,4,5)
set4(100) = false
println(set4)
```
