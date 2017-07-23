## 简单的模式匹配
```
def f1(count:Any):Unit = {
    count match {
        case 1 => {
        println(s"value=1")
        }
        case 2 => {
        println(s"value=2")
        }
        case 3 => println(s"value=3")
        case e =>{
        //匹配任何值，并将值赋值给变量e
        println(s"value=${e}")
        }
    }
}

f1("213123")
```

## 异常匹配
```
def f2(throwable: Throwable): Unit = {
    throwable match {
        case e: IOException => {
        // 处理IO异常
        println(s"IO异常:${e.getMessage}")
        }
        case e: IllegalArgumentException => {
        println(s"参数异常:${e.getMessage}")
        }
        case e: RuntimeException => {
        println(s"运行时异常:${e.getMessage}")
        }
        case _ => {
        // 当不需要匹配值的时候，可以使用下划线代替匹配值, 表示匹配任何值
        println(s"其它异常:${throwable.getMessage}")
        }
    }
}

try{
    3 / 0
} catch {
    case e => f2(e)
}

try {
    throw new IOException("file not exists")
} catch {
    case e => f2(e)
}

try {
    throw new Exception("exception")
} catch {
    case e => f2(e)
}
```

## 小例子list求和
```
def f3(list:List[Int]):Int = {
    val sum = list match {
        case Nil => 0
        case head :: Nil => head
        case head :: tail => head + f3(tail)
    }
    sum
}

val list = List(1,3,5,6,7,8,9)
println(list.head :: list.tail)
println(s"sum=${list.sum}")
println(s"sum2=${f3(list)}")
```

## case Class的模式匹配使用
```
//声明类
case class Student(
    var name:String,
    age:Int
)

/**
* case class的匹配
*
* @param student
*/
def f4(student: Student): Unit = {
    student match {
      case Student(name, 20) => {
        println(s"20=${name}")
      }
      case Student(name, age) if age > 15 => {
        println(s">15:${name}")
      }
      case Student(name, age) => {
        println(s"${name}:${age}")
      }
    }
}
f4(Student("gerry", 20))
f4(Student("gerry", 18))
f4(Student("gerry", 10))
```

## list的一些用法
```
 // Option: Scala中有值和无值的一种表示类，有两个子类，分别是：Some-->有值，None-->无值
    val map = Map(1 -> "a", 2 -> "b")
    val values: Option[String] = map.get(1)
    values match {
      case Some(v) => println(s"value=${v}")
      case None => println("无值")
    }

    // list map 偏函数
    val list2 = List(1 -> "a", 2 -> "b")
    list2.map(t => t._2 * t._1).foreach(println)
    list2
      .map {
        case (num, key) if num % 2 == 1 => key * (num + 5)
        case (num, key) => key * (num * 2)
      }
      .foreach(println)
```
