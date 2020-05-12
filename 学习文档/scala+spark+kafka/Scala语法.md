##scala是一门多范式的编程语言：即面向对象和函数式
Scala运行在Java虚拟机上，因此必须要有jre环境


##scala数据结构
1.数组：存放同类型元素
2.集合：可存放不同类型元素


##常量与变量
**常量用val定义 val x:Int=1+1
  常量的类型可以自动推断，因此可以省去显示声明类型，如val x=1+1
**变量用var定义 var y

##代码块
**用{}括起来的多条表达式，由于Scala很少用return关键字，因此代码最后一句是返回值

##方法
**方法的定义 def 方法名(参数列表):返回类型={表达式}
   *def add(a:Int,b:Int):Int={return a+b}
  - 如果方法的最后一句为返回值，则可以去掉return关键字，Scala可以自动完成推断
  - 如果方法体只有一句代码，那么大括号也可去掉，因此上式可写成
   def add(a:Int,b:Int)=a+b
  - 如果一个方法没有返回值，那么返回类型是Unit,等号可以省略
   def printA(msg:String){println(msg)}
 **方法可以接受多个参数列表，也可以没有参数列表
   def addThenMultiply(x: Int, y: Int)(multiplier: Int): Int = (x + y) * multiplier
   println(addThenMultiply(1, 2)(3)) // 
   
   def name: String = System.getProperty("user.name")
   println("Hello, " + name + "!")

##函数
**函数在方法的基础上去掉了def关键字和方法名，并且这个代码块需要别别人调用，因此需要用一个变量来引用
  *var 变量名=（参数列表）=>{表达式}
  例如 var addone=(x:Int) =>x+1
  var addString=(x:String,y:String) =>{x+y}
  
**匿名函数，在Scala中函数绝大多数的使用是匿名函数
 *匿名函数格式： ()=>{}
  *可以将匿名函数，返回给var定义的变量
  *匿名函数不能显示的声明返回类型，Scala会自动推断返回类型
  *当匿名函数中参数只使用了一次，可以用_替代
    //有参匿名函数
       var a=(x:Int,y:Int)=>x+y
       a(1,2)
    //无参匿名函数
       var a=()=>{println("我爱Scala！")}

**递归函数，自己调用自己，必须指出返回类型
  def fun1(x:Int):Int={
  if(x==1)
    x
  else
  x*fun1(x-1)
  }
  println(fun1(5))
  
**可变参数，用*表示  def fun2(element:Int*)
 def fun4(elements :Int*)={
   var sum = 0;
   for(elem <- elements){
      sum += elem
   }
   sum
 }
 println(fun4(1,2,3,4))
 
##函数与方法的区别
函数在Scala里是‘头等公民’，他可以像任何其他数据类型一样被传递和操作，函数是一个对象，有apply,curried,toStrig,tupled这四个方法。
而方法不具备这些特性

##方法转为函数（神奇的下划线）
在方法后面接一个下划线_
 

##循环
*for循环语法结构
for(i <- 表达式/数组/集合)
for(i <- 1 to 10) println(i)

##调用方法和函数
Scala中的+，— * / %等操作符和Java的作用一样，但是特别是：这些操作符实际上是方法
a+b 实际过程是 a.+(b)


##数组，映射，元组，集合
#数组
*定长数组
  val a=new Array[T](n)
*变长数组
  var b= ArrayBuffer[T]() 注意：需要导包  import scala.collection.mutable.ArrayBuffer
*数组添加元素：b+=1  b+=(1,2)
*数组添加数组：b++=Array(1,2)
 
  
  var a=Array(1,2,3,4,5,6)
  a.filter(x=>x%2==0).map(x=>x*10)
等价于
  a.filter(_%2==0).map(_*10)  //神奇的下划线 ：此时代表数组中的每一个元素
  
  
##映射Map
**构建不可变映射
1.val map=Map(key ->value,key->value ....)
2.利用元组构建 val map=Map((key,value),(key,value)...)

*根据k访问value
map1(K)

*访问map中的所有k
map1.keys

*循环遍历map的所有k的value
 for(k <- map1.keys) println(k)

*getOrElse方法 
map1.getOrElse("wangwu",50)  //没有该K就进行添加
 
**构建可变映射
val map=new HashMap[String,Int]

1.添加元素
map +=("mayaun"->50)  
map +=("mayhua"->49,"chen",34)

2.删除元素
map -=("mayaun") 
map.remove("") 


##类
**Scala中的object
1.存放工具方法和常量
2.单例模式，可以不用new，直接静态调用里面的属性和方法

**Scala中的伴生对象
如果有一个class对象，还有一个和class同名的object文件，那么就称这个object是class的伴生对象，class为object的伴生类
特点：可以互相访问

**apply方法
可以不用new关键字来生成对象，可以直接用类名(参数)生成实例对象

**main方法
1.作为程序的入口，必须放在object代码块中
2.可以通过extend APP代替main方法

##模式匹配
match  case

##偏函数

 
  
  
  
