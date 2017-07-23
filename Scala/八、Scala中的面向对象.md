Scala中的面向对象
* 作用域：可以放在属性、方法、class、object、trait上
  * public：默认，而且public不能写
  *  protected：功能和java一样，但是scala不推荐使用
  * private: 功能和java一样
  * private[包名/this]: 分别表示包可用、当前类对象可用

* class：类
  类似于java中的类(单继承等等)
* abstract class：抽象类
  类似于java中的抽象类
* object：对象
  scala中不支持static关键字 类似于java中单例对象，在jvm中只存在一份；所以object中的所有属性、方法均可以看成是staitc关键字修饰的
* trait: 特质 
  当做接口来使用

## 声明一个类
```
class People{
  var name:String = ""
  val age:Int = 10

  def eat():String = {
    name + " eat..."
  }

  def watchFootball(teamName:String){
    println(name + " is watching match of " + teamName)
  }
}
```

## 调用一个类
```
object SimpleObjectDemo {
  def main(args:Array[String]){
     val people = new People()
     people.name = "kaka"
     println("name is :" + people.name)
     println("name is :" + people.name + "，age is " + people.age)

     //当调用的方法没有入参是，可以不用带括号
     println("invoke eat method：" + people.eat)
     println("invoke eat method：" + people.eat())

     println("invoke watchFootball method : " + people.watchFootball("Chelsea"))
  }
}
```

## 构造函数
```
Scala中的类的继承是单继承的和java一样，构造的顺序和java一样   
class的构造函数
  默认构造函数为空
  主构造函数是class类名后定义的一个括号 + class的{}中的所有内容
  辅助构造函数是以this为名称的，而且没有返回值的方法/函数, 辅助构造函数必须调用主构造函数，而且必须是第一行
```

## object、伴生对象/伴生类、apply方法、update方法
```
object：
  对象，为了解决scala中不能class中写static的问题的一方案
  在jvm中只会存在一份，可以当做单例对象来使用
  内部可以定义属性、方法等

伴生对象/伴生类：
  当在一个文件中，class的名称和object的名称一致的时候，认为class是object的伴生类，object是class的伴生对象
  伴生类和伴生对象之间可以互相访问private修饰的变量/属性/方法
  
apply方法：
  object中：
    提供了一种便捷的对象构建方式，可以通过object名称以及apply函数的参数列表直接进行对象的构建，不需要使用new关键字
  class中：
    提供的是一种便捷的数据获取/判断操作，类似list(0)

update方法：
  class中：
    提供的一种数据插入、更新的便捷操作，类似arr(0) = 100
```

## case class
```
case class:
  其实就是class和object的一个结合，内部会自动的生成class对应的object对象，并且在object中产生apply方法
  默认的属性是val修饰，可以改为var修饰
  注意：case class定义的时候，属性最多22个
  最常用的一个地方是：模式匹配
```

## trait、继承

trait
  当做Java的接口来使用<br>
  区别：<br>
  *  可以包含已经实现的方法或者属性
  *  一个类可以继承/实现多个trait

继承：
  Scala中的类是单继承的<br>
  Trait是可以多继承的<br>
  使用关键字extends进行class/trait的继承，多继承的时候使用关键字with

```
trait A

trait B {
  val a = "A"
  var b = "B"

  def fb(): Unit = {
    println("fb invoke")
  }

  val c: Int
  var d: Int

  def gb(): Unit
}

trait C extends A with B

abstract class D extends C {
  override val a: String = "d113"
  override val c: Int = 10
}

class E extends D with A with B with C {
  override var d: Int = 112

  override def gb(): Unit = println("D gb")
}
```

## 异常处理
```
object ExceptionDemo extends App{
  try{
    3 / 0
  }catch{
     case e:ArithmeticException => println(s"除数不能为0：${e.getMessage}")
     case e:RuntimeException => println(e.getClass)
  }finally{
    println("一定会执行")
  }
}
```


