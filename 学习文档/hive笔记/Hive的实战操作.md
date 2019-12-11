# Hive的实战操作

---

### Client模式
执行hive命令启动client

##### 数据库的创建和使用

    show databases;          --显示数据库列表
    create database badou;   --创建badou数据库
    use badou;               --进入badou数据库
    drop database badou;     --删除badou数据库
    create schema badou;     --创建badou数据库

##### 表的操作和使用

    show tables;                           --显示所有表
    create table article(sentence string); --创建article表(默认内部表)
    desc article;                          --显示表结构
    drop table article;                    --删除表

##### 插入单条记录

    insert into article(sentence) values ('THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS');

##### 数据分割

    select explode(split(sentence,' ')) word from article;

##### wordcount计数

	select word,count(*) cnt
    from (
    select explode(split(sentence,' ')) word from article
    ) t
    group by word
    order by cnt desc;

##### 创建内部表并导入数据

	create table article(sentence string)
    row format delimited fields terminated by '\n';

##### 导入本地数据(相当于put本地文件到HDFS上)

	load data local inpath '/opt/mapreduce/demo/mybatis_NOTICE.txt'
    into table article;

##### 创建外部表并导入数据

	create external table article_ext(sentence string)
    row format delimited fields terminated by '\n'
    stored as textfile 
	location '/user/hive/warehouse/xie.db';

    -- stored as textfile 创建表默认存储方式是文本格式
    -- location '/data/ext' 相当于当前外部表的warehouse
    -- 删除表时，/data/ext 目录下的数据不会被删除

##### 创建分区表
    -- 以dt字段分区
	create table article_dt(sentence string)
    partition by(dt string)
    row format delimited fields terminated by '\n';
	
	-- 导入数据
    insert overwrite table article_dt partition(dt='20180930') select * from article limit 17;
    insert into table article_dt partition(dt='20180929') select * from article limit 34;
    -- overwrite 覆盖原数据
    -- into 新增数据
    -- load本地文件也可以

	--查看分区
	show partitions article_dt;
    select * from article_dt where dt between '20180920' and '20181001';

##### 创建分桶表
	-- 先创建一个表，导入数据
    create table udata(
    user_id string,
    item_id string,
    rating string,
    `timestamp` string
    )row format delimited fields terminated by '\t';
	
    load data local inpath '/opt/data/u.data' 
    into table udata;
	-- 加local表示本地路径
	-- 不加local表示hdfs路径

	-- 创建分桶表
    create table bucket_users(
    user_id string,
    item_id string,
    rating string,
    `timestamp` string
    )clustered by(user_id) into 4 buckets;
	
	--在将数据插入分桶表之前还需要设置两个属性
	--set hive.enforce.bucketing=true;
    --set mapreduce.job.reduces=-1;
	-- 将udata表数据导入分桶表中，用load data 方式加载的不能分桶
    insert overwrite table bucket_users
    select cast(user_id as int) as user_id,item_id,rating,`timestamp` from udata;
	-- 此时数据以user_id对4取模分成四份

##### Hive的优化
	--设置每个reduce处理数据的大小(优先级最低)
    set hive.exec.reducers.bytes.per.reducer=<number>
	--设置reduce最大数(优先级其次)
    set hive.exec.reducers.max=<number>
	--设置全局reduce个数(优先级最高)
    set mapreduce.job.reduces=<number>

##### Hive的其他操作
**Multi-insert & multi-group by**

从一份基础表中按照不同的维度，一次组合出不同的数据

    from <from_statement>
        insert overwrite table <tablename1> [partition(partcol1=val1)] <select_statement> group by key1
        insert overwrite table <tablename2> [partition(partcol2=val2)] <select_statement> group by key2

**Automatic merge**

当文件大小比阈值小是，hive启动一个MR进行合并

	hive.merge.mapfiles=true;是否合并map输出文件，默认为true
	hive.merge.mapredfiles=false;是否合并reduce输出文件，默认false
	hive.merge.size.per.task=256*1000*1000;合并文件的大小

**Multi-Count Distinct**

必须设置参数：set hive.groupby.skewindata=true;

	select dt,count(distinct unique_id) from tablename where dt=20181118 group by dt

**Hive的join优化**

一个MR

	select a.val,b.val,c.val 
    from a 
    join b on a.key=b.key 
    join c on a.key=c.key

多个MR

	select a.val,b.val,c.val 
    from a 
    join b on a.key=b.key1 
    join c on c.key=b.key2