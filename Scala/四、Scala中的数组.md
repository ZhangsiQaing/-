# 数组
```
//创建数组（定长数组）
val arr1 = Array(1,2,3,4,5)
val arr2 = new Array[Int](5)

//获取和修改数组值
println(arr1(0))
arr1(0) = 1000

//循环数组
for(elm <- arr1){
    println(elm)
}

//变长数组，可以改变长度
val buf1 = ArrayBuffer[Int]()
// 新增数据（追加的形式）
buf1 += 3
buf1 += 10
buf1 ++= Array(1,2,3,4,5,6)
//给指定位置插入数据
buf1.insert(0,4,5,6)

//删除元素（一次只能删除一个对应的值，如果有重复值，需要多次删除；如果元素不存在不会报错）


//buf和Array的相互转化
val arr3 = buf1.toArray
val buf2 = arr1.toBuffer

//数组中添加不同类型的数据
val arr4 = Array("gerry",18,1234.0)
val name:String = arr4(0).asInstanceOf[String]
val age:Int = arr4(1).asInstanceOf[Int]
val salary = arr4(2).asInstanceOf[Double]
    asInstanceOf //判断是否是给定的数据类型
```



# 元组
* 内部可以放不同数据类型的数据
* 下标从1开始，读取数据方式为：下划线+下标，eg：tuple._1表示获取第一个元素的数据
* 元组中的数据不允许进行修改操作，不允许修改元组的数据引用，至于数据内部是可以进行修改
* 元组中的数据类型可以是任何数据类型
注意：Scala中最多支持22元组
```
 //定义二元组
val tuple1:(String,Int) = ("gerry",18)
val name:String = tuple1._1
val age:Int = tuple1._2
println(s"${name}:${age}")

//定义三元组
val t1 = ("a",(1,2,3),456.23)
println(t1._1)
println(t1._2._1)
```


