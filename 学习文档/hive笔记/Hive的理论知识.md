# Hive的理论知识

---

Hive基于一个统一的查询分析层，通过SQL语句的方式对HDFS上的数据进行查询、统计和分析

### Hive出现的原因
* 对存在HDFS上的文件或Hbase中的表进行查询时，是要手工写一堆Mapreduce代码
* 对于统计任务，只能由懂MapReduce的程序员才能搞定
* 耗时耗力，更多精力没有有效的释放出来

### Hive是什么？
* Hive是一个SQL解析引擎，将SQL转化成MapReduce，然后在Hadoop平台上运行，达到快速开发的目的
* Hive中的表是纯逻辑表，就只有表的定义，即表的元数据，一般元数据存储在mysql上。本质是hadoop的目录/文件，达到了元数据与数据存储分离的目的
* Hive本身不存储数据，它完全依赖HDFS和MapReduce
* Hive的内容是读多写少，不支持对数据的改写和删除
* Hive中没有定义专门的数据格式，由用户指定，需要指定三个属性
    * 列分隔符  空格、逗号、\t
    * 行分隔符  \n
    * 读取文件数据的方法

### Hive中的SQL和传统SQL的区别

||HQL|SQL|
|------|------|------|
|数据存储|HDFS或Hbase|Local FS(本地文件系统)|
|数据格式|用户自定义|系统决定|
|执行|MapReduce|Executor|
|执行延迟|执行MR延时高|延时低|
|可扩展性|高|低|
|数据规模|高|低|
|数据检查|读时模式|SQL写时模式|
|数据更新|不支持(新版本支持数据覆盖)|支持|
|索引|有(0.8版本以后增加)|有|

### Hive语句转换过程
* **解析器**：生成抽象语法树
* **语法分析器**：验证查询语句
* **逻辑计划生成器(优化器)**：生成操作符树
* **查询计划生成器**：转换为map-reduce任务

### Hive执行过程
* 用户发送一个查询请求访问Driver
* Driver首先得到一个计划，然后进行编译
* 编译的过程会查询元数据，确定是否有需要查询的列，通过之后返回Driver的计划
* 执行计划，需要一个进程，这个过程需要与元数据进行交互
* 通过进程转化成mapreduce任务提交给jobtracker或者yarn

### Hive的内部表和外部表
* 内部表数据存储在HDFS的/user/hive/warehouse目录下，外部表数据并没有移动到自己的数据仓库下，也就是外部表的数据不是它自己管理。
* 在删除内部表的时候，Hive将会删除属于表的元数据和数据全部删掉；而删除外部表的时候，Hive仅仅删除外部表的元数据，数据不会删除。
* 原始数据建表为外部表；可以用外部表作分析，内部表进行操作
### 内外部表的互相转化
*修改内部表为外部表  alter table 表名 set tblproperties('EXTERNAL'='TRUE');
*修改外部表为内部表  alter table 表名 set tblproperties('EXTERNAL'='FALSE');


### Hive的分区(partition)
*分区表 partitioned(mt string,dt string,...) 后面写几个就对应几个目录  /user/hive/warehouse/xie.db/book_borrow/mt=20190729/dt=0729
*
* partition是辅助查询，缩小查询范围，加快数据的检索速度和对数据按照一定的规格和条件进行管理

### Hive的分桶(bucket)
* hive中的table可以拆分成partition，table和partition可以通过‘clustered by’进一步分bucket，bucket中的数据可以通过‘sort by’排序。
* set hive.enforce.bucketing=true;这个参数必须为true才可以分桶，正常分桶后table或partition对应的目录下有bucket个数的文件
* bucket主要的作用：
	* 数据采样
	* 提升某些查询操作效率 
* 数据采样
	* tablesample是抽样语句，语法tablesample(bucket 1 out of 4 on user_id);
	* y必须是table总bucket数的倍数或者因子，相当于将总的bucket数平均再分成y份，取其中一份，x表示从第几份开始取。例如总bucket数是4，y=8，相当于每一份是一个bucket的1/2，如果y=2，相当于每一份是2个bucket，此时取1和3桶
	* 原表采样，抽取90%左右的数据select count(*) from udata where user_id%9!=0;
	
### Hive数据类型
* 原生类型
	* TINYINT
	* SMALLINT
	* INT
	* BIGINT
	* BOOLEAN
	* FLOAT
	* DOUBLE
	* STRING
	* BINARY
	* TIMESTAMP
* 复合类型
	* Arrays
	* Maps
	* Structs
	* Union

### Hive的优化
##### Map的优化
* 作业会通过input的目录产生一个或者多个map任务
	* set dfs.block.size
* Map越多越好吗？是不是保证每个map处理接近文件快的大小？
* 如何合并小文件数，减少map数
	* set mapred.max.split.size=100000000;
	* set maperd.min.split.size.per.node=100000000;
	* set mapred.min.split.size.per.rack=100000000;
	* set hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;
* 如何适当的增加map数
	* set mapred.map.tasks=10;
* Map端聚合,MR中的Combiners
	* set hive.map.aggr=true

Map的优化主要是在文件数和文件大小上面，默认一个文件一个map，在小文件比较多的情况下可以手动减少Map的数量，降低资源消耗;或者限定一个map处理的大小;也可以做Combiner聚合

##### Reduce的优化
* 调整reduce的个数
	* set hive.exec.reducers.bytes.per.reducer=100000000;
	* set mapred.reduce.tasks=10;
	* set hive.exec.reducers.max=3;
	* set mapreduce.job.reduces=5;
* 一个reduce的情况
	* 没有group by
	* order by
	* 笛卡尔积（例如一位数数组做组成两位数的排列）
* 分区裁剪
	* where中的分区条件会提前生效，比如dt分区，不必特意做子查询，直接join和group by
* 笛卡尔积
	* join的时候不加on条件或者无效的on条件，Hive只能使用1个reduce来完成笛卡尔积
* Map join
	* /*+ MAPJOIN(tablelist) */，必须是小表，不要超过1G，或者50万条记录，相当于把小表数据cache到内存中，省去reduce过程
* Union all/distinct
	* 先做union all再做join或group by等操作可以有效减少MR过程，尽管多个select，最终只有一个MR。(union并集做去重，union all直接拼接不去重)

##### join优化的表连接顺序
* 按照join顺序中的最后一个表应该尽量是大表，因为join前一阶段生成的数据会存在于reduce的buffer中，通过stream最后面的表，直接从reduce的buffer中读取已经缓冲的中间结果数据(这个中间数据可能是join顺序中，前面表连接的结果的key，数据量相对较小，内存开销就小)，这样，与后面的大表连接时，只需要从buffer中读取缓存的key，与大表中的指定key进行连接，速度会更快，也可能避免内存缓冲区溢出
* 使用hint的方式启动join操作

a表被视为大表(小表比较多指定大表)，先进行c.key=b.key1的连接，数据会cache到内存中，只进行map，没有进行shuffle

	select /*+ STREAMTABLE(a)*/a.val,b.val,c.val
    from a 
    join b on a.key=b.key1 
    join c on c.key=b.key1

MAPJOIN会把小表全部读入内存中(大表比较多指定小表)，在map阶段直接拿另外一个表的数据和内存中表数据做匹配，由于在map是进行了join操作，省去了reduce运行，效率也会高很多

	select /*+ MAPJOIN(b)*/a.key,a.value
    from a
    join b on a.key=b.key;

* 左连接时，左表中出现的join字段都保留，右表没有连接上的都为空

执行顺序是，首先完成两表的join操作，然后进行where过滤，这样在join过程中输出大量结果，再对这些结果进行过滤，比较耗时。可以优化，把where的条件放在on里面，这样在join过程中不满足条件的优先过滤掉

	select a.val,b.val
    from a
    left outer join b on a.key=b.key
    where a.dt='20181110' and b.dt='20181111'

	
	select a.val,b.val
    from a
    left outer join b on (a.key=b.key
    and a.dt='20181110' and b.dt='20181111')

##### 并行执行

同步执行hive的多个阶段，hive的执行过程中，将一个查询转化成一个或者多个阶段，某个特定的job可能包含众多的阶段，而这些阶段可能并非完全相互依赖，也就是说可以并行执行的，这样可能使得整个job的执行时间缩短。hive执行开启：set hive.exec.parallel=true

##### 数据倾斜问题

**案例1**：当落到一个reduce中的数据非常多，会造成数据倾斜，其他reduce结束了，这个reduce会一直跑下去

**解决方案**：

* combiner 
	* 即在map端进行聚合，避免数据倾斜发生
* 一个MR任务拆分成两个(万能方法)
	* set hive.groupby.skewindata=true

**案例2**：select * from dw_log t1 join dw_user t2 on t1.user_id=t2.user_id

**现象**：两个表都是上千万

**思路**：当天登录的用户很少

**解决方案**：

	select /*+MAPJOIN(t12)*/ * from dw_log t11
    join (
        select /*+MAPJOIN(t)*/ t1.* from (
            select distinct user_id from dw_log
        ) t
        join dw_user t1 on t.user_id=t1.user_id
    ) t12
    on t11.user_id=t12.user_id

**案例3**：select day,count(distinct session_id),count(distinct user_id) from log group by day

**问题**：同一个reduce进行distinct操作时压力很大

**解决方案**：

	select day,
		count(case when type='session' then 1 else null end) as seesion_cnt,
		count(case when type='user' then 1 else null end) as user_cnt,
	from (
		select day,session_id,type
		from (
			select day,session_id,'session' as type
			from log
			union all
			select day,user_id,'user' as type
			from log
		) t1
		group by day,session_id,type
	) t2
	group by day