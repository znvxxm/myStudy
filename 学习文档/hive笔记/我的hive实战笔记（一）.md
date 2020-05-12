###我的hive实战(1)

---

##hive添加、修改、删除字段操作
*添加新的字段和注释
   alter table bi.mei_ike_trans_monthly_detail_i_m ADD COLUMNS (currency STRING COMMENT 'BIZHONG');//括号是必须的
*在指定地方添加字段
   alter table table_name add columns (c_time string comment '当前时间'); -- 正确，添加在最后
   alter table table_name change c_time c_time string after address ;  -- 正确，移动到指定位置,address字段的后面
*更改字段
   Alter table 表名  change column 原字段名称  现字段名称  数据类型;
   
##hive 修改表名
alter table table_name rename to new_table_name;


##hive删除操作
**默认的hive不支持delete和updata操作
*--  删除分区
alter table employee_table drop partition (stat_year_month>='2018-01');
alter table ods_data_label_of_ods_power_original_data drop partition (mt='2020');

*--  按条件删除数据
insert overwrite table employee_table select * from employee_table where id>'180203a15f';


##hive 函数
**lateral view 与 explode函数
*lateral view一般与explode函数联合使用。explode为列转行函数，结合split将一个字段按照分隔符切开成多条数据： explode(split(is_valid,','))
*lateral view 是侧视图，相当于一个虚拟表与源表笛卡尔积关联
             select goods_id2,sale_info,area2
             from explode_lateral_view 
             LATERAL VIEW explode(split(goods_id,','))goods as goods_id2 
             LATERAL VIEW explode(split(area,','))area as area2;

**NVL函数
*多表相关联时会出现null的情况，将null用其它值替换可用NVL函数
*NVL(expr1, expr2)： 
    1、空值转换函数； 
    2、类似于mysql-nullif(expr1, expr2)，sqlserver-ifnull(expr1, expr2)。
    
    备注： 
    1、如果expr1为NULL，返回值为 expr2，否则返回expr1。 
    2、适用于数字型、字符型和日期型，但是 expr1和expr2的数据类型必须为同类型。


##hive表格基础设置
**中文乱码
*MySQL metastore
     alter table COLUMNS_V2 modify column COMMENT varchar(256) character set utf8;
     alter table TABLE_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
     alter table PARTITION_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
     alter table PARTITION_KEYS modify column PKEY_COMMENT varchar(4000) character set utf8;
     alter table INDEX_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
*hive-site
     <property>
               <name>javax.jdo.option.ConnectionURL</name>
               <value>jdbc:mysql://10.45.157.131:3306/metastore?createDatabaseIfNotExist=true&amp;characterEncoding=UTF-8</value>         
               <description>JDBC connect string for a JDBC metastore</description>
     </property>

**显示字段
*hive-site
      <property>
          <name>hive.cli.print.header</name>
          <value>true</value>
        </property>
        <property>
          <name>hive.resultset.use.unique.column.names</name>
          <value>false</value>
        </property>

**字段与数据对不齐
*beeline -u jdbc:hive2://**.**.***.**:10000 -n hadoop 使用beeline进行连接


##Hive sql比较大小
对于string类型的value要比较数值的大小，必须要将string类型转化成int或者其他可以比较的类型，否则结果会按字符串比较大小








