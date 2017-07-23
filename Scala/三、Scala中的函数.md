# 函数
定义：在Scala中，函数与类、对象等一样，都是一等公民。Scala中的函数可以独立存在，不需要依赖任何类和对象


## 函数简单定义
```
def 方法名(参数名：参数类型，...) ：返回类型 = {
}

#栗子
def add() = {
   println("say hello")
}
add()
add  //当调用的函数没有参数，方法的括号也可以省略

#在函数中定义函数
def fun1(a:Int){
    def fun2(b:Int): Unit ={
    println("fun1 ...fun2...：" + (a + b))
    }
    fun2(100)
}
fun1(5)
```

### 默认参数
```
def sayName(name:String = "zhangsan"){
    println(name)
}
sayName()
sayName("lisi")
```

### 变长参数
```
//String*表示接收一系列的String类型的值，类似于java中的可变参数
def printCourses(c:String*) = {
    c.foreach(x => println(x))
}
printCourses("hadoop","hive","hbase","spark")
printCourses("spark","storm")

//传入数组
var arr = Array("hadoop","hive","habase","spark")
printCourses(arr:_*)
```



## 匿名函数
### 定义匿名函数
```
val sayHelloFunc = (name:String) => println("Hello,"+name)
def add = (x:Int,y:Int) => {
    x + y
}
println(add(2,3))

def add3(x:Int) = x + 100
println(add3(5))

val add4 = add3 _  //函数可以当成常量/变量进行传递
println(add3(44))
```

## 高阶函数
### 高阶函数简单使用
```
def greeting(name:String,sayFunc:(String) => Unit):Unit = {
      sayFunc(name)
}
def sayHiFunc(name: String) = println(s"Hi,${name}")
greeting("name1",sayHiFunc)
greeting("xiaoliu",sayHelloFunc)

// 3*_的语法必须掌握，spark源码大量使用
def triple(func:(Int)=>Int) = {func(3)}
triple(3*_)  //这里的_指代3
```

### Scala的常用高阶函数
```
map:对传入的每个元素都进行映射，返回一个处理后的元素
Array(1,2,3,4,5).map(2*_)

foreach:对传入的每个元素都进行处理，但是没有返回值
(1 to 9).map("*" * _).foreach(println _)

filter:对传入的每个元素都进行条件判断，如果对元素返回true,则保留该元素，否则过滤掉该元素
(1 to 20).filter(_ % 2 == 0)

reduceLeft:从左侧元素开始，进行reduce操作
//下面这个操作就相当于1*2*3*4*5*6*7*8*9
(1 to 9).reduceLeft(_ * _)
```

### 递归
  在函数内部调用函数自身进行解决问题<br>
  每一次递归会将允许代码添加到栈中，如果递归的深度太深的话，有可能出现OOM的异常<br>
尾递归：对递归的一种优化方式，不会将每次的递归放到栈中<br>
  要求：最后返回的实递归代码
```
def f(n:Int):Int = {
  if (n <= 1) 1
  else f(n - 1) * n
}

def f(n:Int, m:Int = 1):Int = {
  if (n <= 1) n * m
  else f(n - 1, m * n)
}
  
def f(n:Int):Int = {
  if (n <= 1) throw new RuntimeException("1")
  else f(n - 1) * n
}

def f1(n:Int, m:Int = 1):Int = {
  if (n <= 1) throw new RuntimeException("1")
  else f1(n - 1, m * n)
}
```






