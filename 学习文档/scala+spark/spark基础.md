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

RDD:不可变、可分区、分布式的弹性数据集
1、RDD是不可变的，如果需要在一个RDD上进行转换操作，则会生成一个新的RDD

2、RDD是分区的，RDD里面的具体数据是分布在多台机器上的Executor里面的。堆内内存和堆外内存+磁盘。

3、RDD是弹性的：

a、存储：Spark会根据用户的配置或者当前Spark的应用运行情况去自动将RDD的数据缓存到内存或者磁盘。它是一个对用户不可见的封装的功能。

b、容错：当你的RDD数据被删除或者丢失的时候，可以通过血统或者检查点机制恢复数据，这个对用户透明的。

c、计算：计算是分层的，有应用->Job->Stage->TaskSet->Task每一层都有对应的计算的保障与重复机制，保障你的计算不会由于一些突发因素而终止。

d、分片：可以根据业务需求或者一些算子来重新调整RDD中的数据分布。



RDD是spark中最基础，也是最重要的的一个概念，简单总结如下

1、RDD是spark提供的核心 抽象 ，全称为Resillient Distributed Dataset，即弹性分布式数据集。
2、RDD在抽象上来说是一种元素集合，包含了数据。它是被分区的。分为多个分区，每个分区分布在集群中的不同节点上，从而让RDD中的数据可以被并行操作；（分布式）
3、RDD通常通过Hadoop上的文件，即HDFS文件或者Hive表，来进行创建；有时也可以通过应用程序中的集合来创建；
4、RDD最重要的特性就是，提供了 容错性，可以自动从节点失败中恢复过来。即如果某个节点上的RDD partition，因为节点故障，导致数据丢了，那么RDD会自动通过自己的数据来源重新计算该partition。这一切对使用者是透明的
5、RDD的数据默认情况下存放在内存中的，但是在内存资源不足时，Spark会自动将RDD数据写入磁盘，对用户是透明的。（弹性）

作者：张凯_9908
链接：https://www.jianshu.com/p/3aaeecbfce82
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


spark的核心编程是什么？其实，就是：

1、定义初始的RDD，就是说，你要定义第一个RDD从哪里读取数据，hdfs、hive、linux本地文件、程序中的集合
2、定义对RDD的计算操作，这个在spark里称之为算子，比如，map、reduce、flatMap、groupByKey，比MR提供的map和reduce强大的多了
3、其实就是循环往复的过程，第一个计算完了以后，数据可能就会到了新的一批节点上，也就是变成一个新的RDD，然后再次反复，针对新的RDD定义计算操作。。。
4、最后，获得最终的数据，将数据保存起来


1.初始化sparkcontext
2.定义初始RDD，即从哪里读取到的数据
3.对初始RDD进行算子操作，即迭代，产生新的RDD，不断反复
4.对结果RDD进行输出

datafile--RDD0--RDD1--RDD2.....RDD_finnal

**RDD操作和持久化

Spark支持两种RDD操作，transformation和action。transformation操作会针对已有的RDD创建一个新的RDD；
而action则主要是对RDD进行最后的操作，比如遍历、reduce、保存到文件等，并可以返回结果给Driver程序。
Transformation的特点就是lazy特性。lazy特性是，如果一个spark应用中只定义了transformation操作，那么即使你执行该应用，
这些操作也不会执行，也就是说，transformation是不会触发spark程序的执行的，他们只是记录了对RDD的操作，但是不会自发的执行；
只有当transformation之后，接着执行了一个action操作，那么所有的transformation才会执行。
Spark通过这种lazy特性，来进行底层的spark应用执行的优化，避免产生过多的中间结果。

action操作执行，会触发一个spark job的运行，从而触发这个action之前所有的transformation的执行，


Spark持久化

Spark非常重要的一个功能特性就是可以将RDD持久化在内存中。当对RDD执行持久化操作时，每个节点都会将自己操作的RDD的pattition持久化到内存中，
并且在之后对该RDD的反复使用中，直接使用内存缓存的partition。这样的话，对于针对一个RDD反复执行多个操作的场景，就只要对RDD计算一次即可，
后面直接使用该RDD，而不需要反复计算多次该RDD。
官方文档上说，巧妙使用RDD持久化，甚至在某些场景下，可以将spark应用程序的性能提升10倍。对于迭代式算法和快速交互式应用来说，RDD持久化，是非常重要的。
要持久化一个RDD，只要调用cache()或者persist()方法即可，在该RDD第一次被计算出来时，就会被直接缓存在每个节点中。
而且spark持久化机制是自动容错的，如果持久化的RDD的任何partition丢失了，那么spark会自动通过其源RDD，使用transformation操作重新计算该partition。
spark自己也会在shuffle操作时，进行数据的持久化，比如写入磁盘，只要是wield在节点失败时，避免需要重新计算整个过程。


**常用transtormation
map      将RDD的每个元素传入自定义函数，获取一个新的元素，然后用新的函数组成新的RDD。(因此，map本身就可以遍历数组集合中的每个元素)
flatMap  与map类似，但是对每个元素可以返回一个或者多个值




























 
