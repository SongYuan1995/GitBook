## DDl

##### 创建数据库

```mysql
create database if not exists db_hive;
create database if not exists db_hive location 'db_hive.db';
```

##### 修改数据库

数据库的其他元数据信息都是不可更改的，包括数据库名和数据库所在的目录位置。

```mysql
alter database db_hive set dbproperties('createtime'='20170830');
```

在mysql中查看修改结果

```mysql
 desc database extended db_hive; 
```

##### 查看数据库详情 

```mysql
desc database db_hive; 
```

##### 删除数据库

```mysql
 drop database db_hive; #数据库为空
 drop database db_hive cascade; #数据库不为空
```



##### 创建表

```mysql
CREATE [EXTERNAL] TABLE [IF NOT EXISTS] table_name  
[(col_name data_type [COMMENT col_comment], ...)]  
[COMMENT table_comment]  
[PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]  
[CLUSTERED BY (col_name, col_name, ...)  
[SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]  
[ROW FORMAT row_format]  
[STORED AS file_format]  
[LOCATION hdfs_path]
#ROW FORMAT  
#DELIMITED [FIELDS TERMINATED BY char] 
#[COLLECTION ITEMS TERMINATED BY char]  
#[MAP KEYS TERMINATED BY char] 
#[LINES TERMINATED BY char]  
#SERDE serde_name [WITH SERDEPROPERTIES (property_name=property_value, 
#property_name=property_value, ...)] 

#SERDE:序列化与反序列化
#STORED AS 指定存储文件类型 ：
#SEQUENCEFILE（二进制序列文件）、TEXTFILE（文本）、RCFILE（列式存储格式文件） 
```

##### 管理表与外部表的互相转换

`alter table stu set tblproperties('EXTERNAL=FALSE');`

##### 管理表：

也被称为内部表， 当我们删除一个管理表时，Hive 也会删除这个表中数据。管理表不适合和其他工具共享数据。

#### 分区表：

对应一个 HDFS 文件系统上的独立的文件夹，该文件夹下是该分区所有的数据文件。因为表是外部表，所以 Hive 并非认为其完全拥有这份数据。删除该表并不会删除掉这份数据，不过描述表的元数据信息会被删除掉。

##### 加载数据到分区表

```mysql
load data local inpath '/opt/module/datas/dept.txt' into table default.dept_partition       partition(month='201709'); 
```

##### 增加分区(多个分区之间用空格间隔)

```mysql
alter table dept_partition add partition(month='201705') partition(month='201704'); 
```

##### 删除分区（多个分区之间用逗号间隔）

```mysql
alter table dept_partition drop partition (month='201705'),partition (month='201706'); 
```

##### 把数据直接上传到分区目录上，让分区表和数据产生关联的两种方式 

###### （1）方式一：上传数据后修复 

```mysql
#上传数据
hive (default)> dfs -mkdir -p /user/hive/warehouse/dept_partition2/month=201709/day=12; 
hive (default)> dfs -put /opt/module/datas/dept.txt  /user/hive/warehouse/dept_partition2/month=201709/day=12; 
#此时查询无数据，需执行修复命令
hive> msck repair table dept_partition2; 
```

###### 方式二：上传数据后添加分区 

```mysql
#上传数据同上
#执行添加分区 
hive (default)> alter table dept_partition2 add partition(month='201709', day='12'); 
```

###### 方式三：上传数据后 load 数据到分区 

```mysql
# 创建目录 
hive (default)> dfs -mkdir -p /user/hive/warehouse/dept_partition2/month=201709/day=10; 
# 上传数据 
hive (default)> load data local inpath '/opt/module/datas/dept.txt' into table 
dept_partition2 partition(month='201709',day='10'); 
```



#### 修改表

##### 重命名

```mysql
# 语法
ALTER TABLE table_name RENAME TO new_table_name 
# 例子
alter table dept_partition2 rename to dept_partition3; 
```

##### 增加/修改/替换列信息

```mysql
# 更新列
ALTER TABLE table_name CHANGE [COLUMN] col_old_name col_new_name column_type 
[COMMENT col_comment] [FIRST|AFTER column_name] 
#例子
 alter table dept_partition change column deptdesc desc int; 
 
 #增加和替换列 
 ALTER TABLE table_name ADD|REPLACE COLUMNS (
     col_name data_type [COMMENT col_comment], ...) 
 # 例子
 alter table dept_partition replace columns(
     deptno string, dname string, loc string); 
```



######  



##### 查询表的类型

`desc formatted stu;`





