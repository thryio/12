# 12DDL是数据定义语言的缩写，简单的说，就是对数据库内部的对象进行创建、删除、修改等操作的语言。）
命令结束符，用“；”或“\g”结束
**********************数据库创建、查询、操作、删除*********************
：dbname为所创建的数据库名称,以下皆是
1 创建数据库
  语法：create database dbname;  
  创建成功提示：Query OK、、、
  未成功（已存在）：ERROR 、、、database exists
2 查询系统数据库
  语法：show databases
  Mysql系统自动创建的4个数据库
  information_schema
  cluster
  mysql
  test
3 操作数据库
  语法：use dbname;
4 查看数据库中创建的所有数据表
  语法：show tables;
5 删除数据库
  语法：drop database dbname;
 
表的创建、查看、删除、修改
 
: "tablename"表示表的名字"，"column_name_" 表示列的名字(比如：名字、姓名、年龄),"column_type_"表示数据类型（比如varchar(10)、date、int(2)），constraints是这个列的约束条件
6 创建一张表
  语法：create table tablename(column_name_1 column_type_1 constraints,column_name_2 column_type_2 constraints,column_name_3 column_type_3 constraints)
7 查看表
  语法：desc tablename;
  *若需要更全面的表定义信息，可以使用：show create table tablename \G;
8 删除表
  语法：drop table tablename
9 修改表
  （1）修改某个表中某个列的某个数据类型
   语法：alter table tablename modify column_name newcolumn_type;
  （2）增加表字段
   语法：alter table tablename add column newcolumn_name newcolumn_type;
  （3）删除表字段
   语法：alter table tablename drop column column_name;
   (4) 字段改名
   语法：alter table tablename change column_name newcolumn_name column_type;(将column_type更改，成为newcolumn_type，可同时更改字段类型)
  （5）修改字段排列顺序
   语法：
①   alter table tablename add newcolumn_name newcolumn_type after column_name;
作用：将newcolumn_name newcolumn_type 加在column_name 后面。
②   alter table tablename modify column_name column_type first;
作用：将newcolumn_name newcolumn_type 放在最前面。
 （6）更改表名
   语法：alter table tablename rename newtablename;
 
*********************DML语句*********************
 
（DML操作是指对数据库中表记录的操作，主要包括表记录的插入、更新、删除和查询、是开发人员日常使用最频繁的操作）
1 插入记录
   语法：insert into tablename(field1,field2,、、、) values (values1,values2,values3、、、);
*：逗号要小写，数据类型要对应
   作用：向表tablename 中插入 values1,values2,values3、、、，对应field1,field2,、、、的顺序。
   *或者不用tablename(field1,field2,、、、)，但是后面的values1,values2,values3、、、必须与字段的排列顺序一样
     若有field未被插入，则为null。
 查看实际插入值语法：select * from tablename;
   插入多条记录：可以在一个value()后面加一个逗号，插入多条语句
2 更新(更改)记录
   语法：update tablename set  field1=value1, field2=value2, field3=value3,where column_name=valuen;
   作用：将 tablename中column_name等于valuen 的field1，field2, field3更改为对应的value1，value2，value3.
   *同时更新多个表中的字段
    语法：update tablename1 type1,tablename2 type2 set  type1.field1=value1,type2.field1=value1 where type1.column_name=type2.column_name;
3 删除记录
   语法：delete from tablename where column_name=valuen(或者其他情况);
   可删除多个表的数据：delete type1,type2,type3、、、from tablename1 type1,tablename2 type2,tablename3 type3、、、where column_name=valuen(或者其他情况)；
4 查询记录
   全部查询语法：select * from tablename;
   (其中*表示将所有记录查询出来，也可以用逗号分割的所有字段代替，如果只想查询其中部分字段，只写相应字段即可)
  （1）查询不重复的记录
   关键词：distinct
   语法：select distinct field from tablename;
  （2）条件查询
   上文中很多地方提到了where关键词，它的作用是根据限定条件查询或者作其他操作，可用类别对应或者数据情况作用条件以便根据需求定位查询目标。
   可用“>、<、>=、<=、!=”等比较运算符，多个条件之间还可以用“and”或者“or”，类似于“&&”和“||”
  （3）排序和限制
   排序
   语法：select * from tablename order by field;（默认由低到高，若想从高到低，则在field后面添加 desc，升序排列是asc，但由于默认，通常不用写。
   同时field后面还可接逗号加其他field，在第一field相同的情况下，按照后面的field进行排序）
   作用：把tablename中的记录按照 field 的高低进行排序显示。
   限制
   语法(例)：select * from tablename order by field limit 3;（显示tablename表中按照field排序后的前3条记录）
   （如果按照排序后的第二条开始的前三条记录，则这样写：select * from tablename order by field limit 1,3;）
   limit和order by经常一起使用
   (4)聚合
   语法：select count(1) from tablename;(统计总数量)
   *统计某个相同类别或者数据类型的数量
   语法：select field ,count(1) from tablename group by field;
   *统计各类别数量又统计总数量
   语法：select field ,count(1) from tablename group by field with rollup;
   *统计数量大于某个数量的类别
   语法：select field ,count(1) from tablename group by field having count(1)>1;(这里举例大于1 的某个类别)
   *统计某个表中最大、最小、数据总和。
   语法：select sum(field),max(field),min(field) from tablename;
   (5)表连接
   *内连接：仅选出两张表中互相匹配的记录
   语法（例）：select field1,field2 from tablename1,tablename2 where tablename1.field=tablename2.field;（查询出两个表中所有field1和field2）
   *外连接：选出其他不匹配的记录
         右连接：包含所有的左边表中的记录甚至是右边表中没有和它匹配的记录。
         语法（例）：select field1,field2 from tablename1 right join tablename2 on tablename1.field=tablename2.field;
         左连接：包含所有的右边表中的记录甚至是左边表中没有和它匹配的记录。
         语法（例）：select field1,field2 from tablename1 left join tablename2 on tablename1.field=tablename2.field;
   （常用内连接）
   (6)子查询
   语法（例）：select from tablename1 where field in(select field from tablename2);
   (查询tablename1中field与tablename2中field相同的类别。其中如果子查询唯一， in 可以用 = 代替)
  （7）记录联合
   语法：select field from tablename2换行
             union all换行
             select field from tablename2;
   field 可以为* 全部显示。
  
**********************DCL语句*********************
  
