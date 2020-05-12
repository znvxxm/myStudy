##Array数组
1.定义：数组是元素的集合，索引从0开始
2.数组声明的两种方式：
  1）var arr1=new Array[String](3)  //通过new关键字，声明一个长度为3，元素类型为string的数组
  
  2）var arr2=Array[Int](1,2,3,4) //直接初始化一个数组，并且将值也放进去
     (1)元素放同一类型：var arr=Array(1,2,3,4)  自动识别数组类型 arr: Array[Int] = Array(1, 2, 3, 4)
     (1)元素放多种类型：var arr=Array(1,2,3.0,“haha”)  自动识别数组类型 arr: Array[Any] = Array(1, 2, 3, 4)

3.对数组元素进行操作：增删改查
   1) Array为定长数组。由于长度已经从创建就开始固定了，因此只能查和改 arr(index)=value
      var a=new Array[Int](3)
    a(0)=4
    a(1)=2
    a(2)=3
   
   2)ArrayBuffer为可变数组,有增删改查
      var arr1=ArrayBuffer[Int]()
      arr1 +=1  //增 给一个变长数组增加一个元素用"+="
      arr1.remove(index)  //删 对指定索引元素进行删除
      arr1(index)=new_value  //改
      arr1(index)=value  //查

4.遍历定长和可变数组 :通过取角标来完成遍历
  for(i <- 0 until arr.length()) {println(arr(i))}
  for 循环中还可以带守卫，即可以加条件 for(i <- arr if arr>0){println(i)}

5.常用算法，内置函数
  arr1.sum   //求和
  arr1.max   //最大
  a.mkString(",") //指定分隔符

6.定长数组和可变长数组互转
    val arr = arrbuff1.toArray //将数组缓冲转换为Array
    val arrbuff2 = arr.toBuffer //将Array转换为数组缓冲

  
##scala数组和java互操作
##scala的IO这块很多都没有自己封装的API,可以用Java的API  ArrayBuffer类似于ArrayList
    var list = new ArrayList[Int](3)
    list.add(100)
    list.add(101)
    list.add(102)
    
    for(i <- 0 until list.size()){
      println(list.get(i))// 循环遍历list数组