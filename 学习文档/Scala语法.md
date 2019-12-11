##scala语法

-
##常量与变量
**常量用val定义 val x：Int=1+1
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
   println(addThenMultiply(1, 2)(3)) // 9
   
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
 
 
  
 
  
  
  
