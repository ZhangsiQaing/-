
# if....else....
if else简单用法
```
val b = 10
if( b > 10){
    print("1111");
}else{
print("2222");
}
```

## while....
这个不说，简单

# 循环
## for循环  
### for循环简单语法
```
val arr = 1 to 10

for(item <- arr){
  print(item + " ");
}
```

### for中加入条件判断
```
val arr = 1 to 10
for(item <- arr if item != 4 && item != 5){
   print(item);
}

for(item <- arr if item < 5){
  print(item);
}
```

### for循环 99乘法口诀表
```
for(item1 <- arr){
  for(item2 <- arr if item1 >= item2){
      print(s"${item1} * ${item2} = ${item1 * item2}  ");
  }
  println("");
}
```

### for绑定临时变量
```
var names = Array("beifeng","  tupian ","   ","   ","  ","  ","   ");
 for(name <- names){
  if(name.trim.nonEmpty){
     println(name.trim);
  }
}

for {
    name <- names
    tmp = name.trim
    if tmp.nonEmpty
}{
    println(tmp);
}
```

### 可以基于for循环创建一个新的集合使用关键字yield
```
val nlist = for(i <- 1 to 10) yield {
    println(s"数据处理{i}");
    i*2
}
nlist.foreach(println);
```

### scala中的breaks
scala不支持break、和continue
但是scala中支持breaks其作用与break一样
```
import scala.util.control.Breaks  //导入包
val a = 1 to 10
  val loop = new Breaks

  loop.breakable {
    for (item <- a) {
      println(item);
      if (item >= 4) {
        loop.break;
      }
    }
  }
```

### Range
* Range是左闭右开区间； [)
* until是左闭右开区间； [)
* to是左闭右闭区间；[]

```
1 to 10  //Range(1,2,3,4,5,6,7,8,9,10)
1.to(10) //点去掉，加空格 等价于 1 to 10

1.until(10)  // Range(1,2,3,4,5,6,7,8,9)

Range(1,10) //Range(1,2,3,4,5,6,7,8,9)
Range(1,10,2) //可以指定步长，Range(1,3,5,7,9)
Range(1,10,4) //Range(1,5,9)
Range(1,10,0) //步长不能为0,java.lang.IllegalArgumentExcepotion:step cannot be 0
Range(9,-1,-1)  //Range(9,8,7,6,5,4,3,2,1,0)
Range(10,1,-1)  //Range(10,9,8,7,6,5,4,3,2)
```
