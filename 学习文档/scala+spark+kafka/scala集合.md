#scala集合可以分为三大类
1.Seq 一组有序集合
2.Set 一组无序且没有重复元素的集合
3.Map 一组K-V对


#Seq代表：Array


#Set
1.不可变 所有语句结果其实都是生成一个新的集合。
    val x = immutable.HashSet[String]("a","c","b")
    val a=x+"d"
    println(a)

2.可变 可以用add进行添加
    val x = new mutable.HashSet[String]()
    x.add("a")
    println(x)


#Map
Map这种数据结构是日常开发中使用非常频繁的一种数据结构。
Map作为一个存储键值对的容器（key－value），其中key值必须是唯一的。 
默认情况下，我们可以通过Map直接创建一个不可变的Map容器对象，这时候容器中的内容是不能改变的。示例代码如下。

  def test() = {
    val peoples = Map("john" -> 19, "Tracy" -> 18, "Lily" -> 20) //不可变
    // people.put("lucy",15) 会出错，因为是不可变集合。
    //遍历方式1
    for(p <- peoples) {
      print(p + "  ") // (john,19)  (Tracy,18)  (Lily,20)
    }
    //遍历方式2
    peoples.foreach(x => {val (k, v) = x; print(k + ":" + v + "  ")}) //john:19  Tracy:18  Lily:20
    //遍历方式3
    peoples.foreach ({ case(k, v) => print(s"key: $k, value: $v  ")})
    //key: john, value: 19  key: Tracy, value: 18  key: Lily, value: 20
  }
上面代码中的hashMap是不可变类型。 
如果要使用可变类型的map，可以使用mutable包中的map相关类。

  def test() = {
    val map = new mutable.HashMap[String, Int]()
    map.put("john", 19) // 因为是可变集合，所以可以put
    map.put("Tracy", 18)
    map.contains("Lily") //false
    val res = getSome(map.get("john"))
    println(res) //Some(19)
  }
 
  def getSome(x:Option[Int]) : Any = {
    x match {
      case Some(s) => s
      case None => "None"
    }
  