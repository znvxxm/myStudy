
##### 数据库的创建和使用

    show databases;                  --显示数据库列表
    create database Eproject;        --创建数据库
    use Eproject;                    --进入数据库
    drop database Eproject;          --删除数据库

##### 表的操作和使用

hadoop fs -ls /user/hive/warehouse

**创建内表
create table if not exists Eproject.app_data_preprocess_label_data_byday1(
    report_date     string comment '数据上报日期',
    device_id       string comment '设备id',
    device_name     string comment '设备名称',
    type            int    comment '数据类型',
    amount          bigint comment '某种类型数据的记录数'
)comment '数据预处理标签数据-日表'
partitioned by (dt string comment '天')
row format delimited 
fields terminated by ','
stored as textfile;

-导入数据
load data local inpath '/home/eproject_text.txt' into table Eproject.app_data_preprocess_label_data_byday1 partition (dt=20191104);
-删除表
drop table Eproject.app_data_preprocess_label_data_byday1;

**创建外表
create external table if not exists Eproject.app_data_preprocess_label_data_byday2(
    report_date     string comment '数据上报日期',
    device_id       string comment '设备id',
    device_name     string comment '设备名称',
    type            int    comment '数据类型',
    amount          bigint comment '某种类型数据的记录数'
)comment '数据预处理标签数据-日表'
partitioned by (dt string comment '天')
row format delimited 
fields terminated by ','
stored as textfile
location '/Eprojectdb'
;

-导入数据
load data local inpath '/home/eproject_text.txt' into table Eproject.app_data_preprocess_label_data_byday2 partition (dt=20191104);
-删除表
drop table Eproject.app_data_preprocess_label_data_byday2;

##hive删除操作
create table if not exists Eproject.app_data_preprocess_label_data_byday3(
    report_date     string comment '数据上报日期',
    device_id       string comment '设备id',
    device_name     string comment '设备名称',
    type            int    comment '数据类型',
    amount          bigint comment '某种类型数据的记录数'
)comment '数据预处理标签数据-日表'
partitioned by (dt string comment '天')
row format delimited 
fields terminated by ','
stored as textfile;

-导入数据
load data local inpath '/home/eproject_text.txt' into table Eproject.app_data_preprocess_label_data_byday3 partition (dt=20191104);

**默认的hive不支持delete和updata操作
*--  删除分区
alter table Eproject.app_data_preprocess_label_data_byday3 drop partition (dt='20191104');

*--  按条件删除数据
insert overwrite table Eproject.app_data_preprocess_label_data_byday3 
select * from Eproject.app_data_preprocess_label_data_byday3 where type>202;

##hive添加、修改字段操作
*添加新的字段和注释
   alter table Eproject.app_data_preprocess_label_data_byday3 ADD COLUMNS (report_time STRING COMMENT '上报时间');//括号是必须的

*在指定地方添加字段
   alter table table_name add columns (mete_id STRING COMMENT '测点ID'); //添加在最后
   alter table table_name change mete_id mete_id string after device_name ;  //移动到指定位置,device_name字段的后面
 
*更改字段
   Alter table 表名  change column 原字段名称  现字段名称  数据类型;
   Alter table Eproject.app_data_preprocess_label_data_byday3  change column amount change_amount int;
   






    show tables;                           --显示所有表
    create table article(sentence string); --创建article表(默认内部表)
    desc article;                          --显示表结构
    drop table article;                    --删除表
