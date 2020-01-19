##spark基础

首先spark是一个框架，一个实时计算框架，可以兼容Java，Python，Scala等开发语言

##两个重要角色
1.Driver
驱动器是执行开发程序中main方法的进程。spark shell，则自动起了一个spark驱动器程序。
如果驱动器终止，则spark应用也结束了。

2.Executor
执行相应的工作进程

##Local本地模式，用于本地学习和调试
Local[K] 指定K个线程
Local[*] 匹配CPU的核数

4040端口为本地模式运行的web访问端口
8088端口为yarn模式的web访问端口

##spark shell
启动spark shell ： spark2-shell

 sc.textFile("/aa.txt").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).collect
 
 map和flatmap相当于是一个加工厂，将集合里面的数据一个个取出来按照指定的函数进行加工后再输出。
 
##IDEA开发spark
*第一次运行spark入门程序WordCount时会报JVM内存不足的错误，需要全局修改JVM的参数
 1.IDEA安装目录--D:\IDEA\IntelliJ IDEA Community Edition 2018.2.8\bin--idea64.exe.vmoptions
 修改Xms=256m和Xmx=1024m(貌似没生效)
 2.直接为项目设置JVM内存：运行箭头左边的下拉单--Edit Configurations--VM options -Xms1024m
 
*开发好的工程需要打包上传，因此工程全路径不能出现中文命名，否则打包失败
 
##Linux下本地模式提交到服务器: (在脚本中配置)

$ /home/hadoop/app/spark/bin/spark2-submit \
--class com.ruozedata.WordCountApp \
--master local[2] \
--name WordCountApp \
/home/hadoop/lib/spark/SparkCodeApp-1.0.jar \
/wc_input/ /wc_output


*spark目录下执行：1.指定类名 2.指定jar包的路径
bin/spark2-submit \
--class com.xie.spark01.WordCount \
/home/myspark_project/WordCount-jar-with-dependencies.jar

##RDD
**什么是RDD
RDD(Resilient Distributed Dataset)弹性分布式数据集，是Spark中最基本的数据抽象。
在代码中具体体现是一个抽象类，代表一个不可变、可分区、里面元素可并行计算的集合。

**算子
1.转化算子
2.行动算子

**创建RDD
1.从集合中创建 makeRDD、parallelize
2.从外部存储中创建

































 
