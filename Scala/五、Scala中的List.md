Scala中的列表

## 创建 
```
//创建不可变List
val l1 = List(1,2,3,4,5,6,7)
val l2 = 1::2::3::4::Nil
val l3 = l1 ::: l2
val l4 = List(Array(1,2,3),Array(7,8,9))

//获取
println(l1(4))
println(l1.head)
println(l1.tail)
println(l1.size)
```

## 可变List
List的增删改查
```
val buf1 = ListBuffer[Int]()

//添加元素
buf1 += 12
buf1 += (12,20,32,33,21,12)
buf1 ++= Array(1,2,3)
buf1 ++= List(4,5,6)

//删除
buf1 -= 12
buf1 --= List(4,5,6)

buf1(0) *= 100
```

## 可变List和不可变List相互转化
```
val buf1 = ListBuffer[Int]()
val buf1 += 12
val l5 = buf1.toList
val buf2 = l5.toBuffer
```

## 常用api

map 返回一个List,每个执行一遍
```
val list1 = List(1,2,3,4,5,6,7,8,9,10)
list1.map(a => a*10).foreach(v => print(v + ":"))
```

flatMap:对集合中的每一个元素进行一次给定函数的执行，要求调用该API的时候给定的函数f，f的返回值必须是集合类型，flatMap返回最终结果是一个集合，集合中的元素类型是给定函数f返回集合中的集合数据类型
def flatMap(f:A => List[B])
```
val list1 = List(1,2,3,4,5,6,7,8,9,10)
val ll2:List[Int] = list1.flatMap(a => (0 to a).toList)
ll2.foreach(v => println(v))
val l2 = 0 to 2
println(l2)
```

filter:数据过滤
```
val list1 = List(1,2,3,4,5,6,7,8,9,10)
list1.filter(_ % 2 == 0).foreach(v => print(s"${v}:"))
```

filterNot:filter求反
```
val list1 = List(1,2,3,4,5,6,7,8,9,10)
list1.filterNot(_ % 2 == 0).foreach(v => print(s"${v}:"))
```


reduce: 对全部数据进行迭代聚合操作， 操作从左往右进行聚合， API返回结果必须是集合中的数据类型,第一个数与第二个数比较，返回结果再与第三个数比较
```
val list1 = List(1,2,3,4,5,6,7,8,9,10)
val r1 = list1.reduce((a,b) => {
   println(s"a=${a},b=${b}")
   a + b
})
println(s"reduce r1 = ${r1}")
```

reduceLeft：功能和reduce基本一样，API返回结果可以是集合中数据类型的父类
```
val r2 = list1.reduceLeft((a,b) => {
      println(s"a=${a},b=${b}")
      a + b
})
println(s"reduce r2 = ${r2}")
```

reduceRight: 功能雷士reduceLeft,区别是计算是从右往左计算的
```
val r3 = list1.reduceRight((a,b) =>{
     println(s"a=${a},b=${b}")
     a + b
})
println(s"reduce r3 = ${r3}")
```

flod:功能类似reduce,区别在于：可以给定一个初始值，初始值类型要求是集合中的数据类型
```
val r4 = list1.fold(100)((a,b) => {
     println(s"a=${a},b=${b}")
     a + b
})
println(s"reduce r4 = ${r4}")
```


foldLeft:功能类似fold,但是初始值类型可以任意
```
val r5 = list1.foldLeft("")((a,b) => {
      println(s"a=${a},b=${b}")
      a + ":" + b
})
println(s"reduce r5 = ${r5}")
```

foldRight:功能类似foldLeft,区别是从右往左进行计算
```
val r6 = list1.foldRight("")((a,b) => {
      println(s"a=${a},b=${b}")
      a + ":" + b
})
println(s"reduce r6 = ${r6}")
```

sortBy
```
list1.sortBy(v => (v + 1) % 2).foreach(v => print(v + ":"))
```

groupBy
```
list1.groupBy(_ % 2).foreach(println)
```

计算每个单词个数
```
import scala.collection.mutable
val lines= List(
      "hadoop,spark,hbase,hive",
      "hadoop,spark,hbase,hive",
      "",
      "spark,hive,spark,spark',hdfs,wordCount"
)

val map = mutable.Map[String,Int]()
val n = lines.mkString("").replace("'","").split("[,]")
for(elm <- n){
if(map.contains(elm)){
      map += (elm -> (map(elm)+1))
}else{
      map += (elm -> 1)
}
for((key,value) <- map ){
      println(s"${key}:${value}")
}
```