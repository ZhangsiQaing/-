## 数据类型
*  Byte
*  Short
*  Int
*  Long
*  Char
*  String
*  Float
*  Double
*  Boolean


## 值与变量
  val和var的区别?
  ```
  val: 值，赋值后，数据不可变
  var: 变量，赋值后，数据可变
  定义格式：
    [var or val] name[:type] = [表达式或者值/对象或者下划线]

    例如：
    var name:String = 'zhang'
    val name1:String = 'zhang'

    val money:Int = 100
    money = 200   //报错

    var name:String = "hello"
    name = "world" //无报错

    name = "welcome"  //一般不需要显示指定类型，因为可以从赋值中判断出类型
  ```
  (推荐使用val，第一选择使用val，如果业务需要，才允许使用var)


## 懒加载
懒加载：lazy,指变量在第一次进行使用的时候进行初始化操作，而且只有在第一次调用的时候会进行初始化<br />
Scala中默认一行一条语句，不需要使用分号";"进行代码的格式化操作， 针对跨行的语句，会自动进行推断，但是要求能够进行推断出来
```
lazy val c = {
   println("nihao")
   12 + 3
}
```

返回值：Scala中的返回值以代码块/表达式的最后一个执行语句的结果作为返回结果，但是scala保留了return的含义，但是不推荐使用

Unit: 类似java中的void，表示空返回

# 下划线
* 变量的初始化值
* 将函数赋值给一个变量的时候，表示赋值的是函数本身而不是函数的执行结果
* 在变成参数函数调用的时候，如果给定的是集合，表示将集合中的每个元素单独的传入函数中，而不是将集合作为一个整体传入
* 在高阶函数的调用过程中，可以使用下划线来代替参数列表
* 在导入包的时候，下划线表示保重的所有类或者子包







