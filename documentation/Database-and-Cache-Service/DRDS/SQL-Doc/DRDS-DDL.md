# DRDS DDL SQL 语法

## 创建拆分表
DRDS的自动分库分表是采用拆分表来实现的，即将原先的一个大表拆分为分布在多个数据库中的分表，每个分表只存储一部分数据，从而提高了对表增删改查的性能。 

虽然DRDS将一个大表拆分成了多个小表，但这对用户是透明的，用户还是可以使用原先的表名访问所有分表中的数据，
DRDS会根据SQL语句中拆分字段的值来判断将SQL语句发往哪个分表中执行。

因此，在DRDS中定义一个拆分表跟传统的非拆分表相比，主要有两个关键的地方需要定义：
- 拆分字段：使用哪个字段对表中的数据进行拆分。
- 拆分函数：使用什么算法对表中的数据进行拆分。


下面是创建拆分表的具体语法

**注意 [DRDS Partition Optiosn] 部分的语法必须在放在最后**
```SQL
CREATE TABLE table_name
(create_definition,...)
[DRDS partition options]
```

**[DRDS Partition Optiosn] 语法**
```SQL
 dbpartition by
     INT_MOD ([column_name])     |
     STRING_HASH ([column_name]) |
     YYYYMM ([column_name]) START ([start_date]) PERIOD [num]|
     YYYY ([column_name]) START ([start_date]) PERIOD [num]  
```
   
### 拆分函数
目前DRDS支持以下的拆分函数，函数名不区分大小写
- INT_MOD(): 对整型字段进行拆分，支持 int，smallint，bigint，tinyint，mediumint
- STRING_HASH()：对字符字段进行拆分，支持 char， varchar
- YYYYMM()：对时间，日期字段进行拆分，按月拆分，支持 timestamp，date，datetime
- YYYY()：时间，日期字段进行拆分，按年拆分，支持 timestamp，date，datetime
 
 **关键字：**（不区分大小写）
 - START ： 按时间拆分时，数据的起始时间，格式为 ‘YYYY’ 或者 ‘YYYY-MM’，其他格式将不被接受，例如start('2018')或start('2018-05')
 - PERIOD：按时间拆分时，每多少时间周期的数据放入到一个分表中，例如每3个月，或每2年的数据放入一个分表中
 
 ### 示例
 1. 按整型字段拆分
  ```SQL
 create table ddl_demo1(
 id int,
 name varchar(10))
 ENGINE=InnoDB DEFAULT CHARSET=utf8
 dbpartition by int_mod(id);
 ```
 
2. 按字符字段拆分
  ```SQL
 create table ddl_demo2(
 id int,
 name varchar(10))
 ENGINE=InnoDB DEFAULT CHARSET=utf8
 dbpartition by string_hash(name);
 ```
 
 3. 使用YYYYMM函数，数据的起始时间为2019年5月，每3个月的数据放入一个分表中
 ```SQL
 create table ddl_demo3(
 order_id int,
 order_date datetime)
 ENGINE=InnoDB DEFAULT CHARSET=utf8
 dbpartition by YYYYMM(order_date) start('2019-05') period 3;
 ```
 4. 使用YYYY函数，数据的起始时间为2000年，每2年的数据放入一个分表中
  ```SQL
 create table ddl_demo4(
 order_id int,
 order_date datetime)
 ENGINE=InnoDB DEFAULT CHARSET=utf8
 dbpartition by YYYY(order_date) start('2000') period 2;
 ```
