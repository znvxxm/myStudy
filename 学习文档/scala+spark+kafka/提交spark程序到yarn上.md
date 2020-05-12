##问题
1.怎么将spark工程打包，打成什么样子的？

pom文件里加入打包插件，不同的插件可以打成不同的包
所用的打包插件最好用以前工程里面的，不要随便百度一个就贴上去
1）瘦包  只是工程的class文件，不含依赖和jar
2）胖包  包含class文件和依赖，打包的形式一般是.tar.gz,解压出来包含jar和lib等

注意：无论是何种打包方式，包打完后需要检测下包是否已经打好
瘦包：通过点击xx.jar--右键打开方式--解压缩--看是否有class文件
胖包：解压，看各文件目录下是否有相应的内容



2.在服务器上的提交命令是什么？
在将spark工程打成jar包后上传，需要用到服务器的spark-submit
注意：spark1.0 是spark-submit  spark2.0 是spark2-submit


提交命令为：
spark2-submit \
--name SparkSqlSchema \
--master local[*] \
--class SparkSqlSchema \
--jars /home/bdp_stream_power/jars/lib/spark-sql_2.11-2.3.0.jar \
/home/bdp_stream_power/jars/src/src-beta-2.0/SparkTest/sparksql04-1.0-SNAPSHOT.jar



--name为指定工程类的路径，在idea中选中类--右键--Copy_Refrence  即可得到类在包中的位置
--jars 为工程依赖的jar包，需要指定在哪里，要给出绝对路径，多个jar包之间用逗号隔开
最后一个为工程jar,前面不需要加任何变量前缀


##流处理提交流程
1.ZNV_E_SparkStreaming_Application编写相应的功能类
2.本地调试完成后，直接打包
3.在服务器相应的路径下建立目录
  如KafkaStreaming类，就要到此路径建立目录/home/bdp_stream_power/jars/src/src-beta-2.0/KafkaStreaming
4.ZNV_E_SparkStreaming_Application-1.0-SNAPSHOT.jar上传到上步建立的目录下
5.在/home/bdp_stream_power/tools目录下建立流处理启动脚本，修改里面对应的参数
