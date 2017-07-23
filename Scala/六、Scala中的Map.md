## Scala中的Map

不可变map集合
```
val map = Map(("gerry" -> 18),("xiaolu",19))
val map2 = Map("gerry" -> 18,"xiaoliu" -> 19)
```

可变map集合
```
val map3 = mutable.Map[Int,String]()
map3 += 1->"a"
map3 += ((1,"a"),(2,"a"))
map3 ++= List((1,"a"),(3,"c"),(5,"e"))
```

map的循环方式
```
map3.foreach(t => {
    println(t._1 + ":" + t._2)
})

for(elem <- map){
    println(s"${elem._1}:${elem._2}")
}

for((key,value) <- map){
    println(s"${key}:${value}")
}

for((key,_) <- map){
    println(s"${key}")
}

for((_,value) <- map){
    println(s"${value}")
}
```