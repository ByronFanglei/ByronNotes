# Mysql:常用命令
## 基础操作
```Mysql
//创建数据库
create database 库名;
//进入数据库
use 库名;
//查看表
show tables;
//创建表
create table 表名(
  //添加主键且自增
id int primary key auto_increment comment "注释",
  //唯一约束(unique)，可以为空，不能重复
name varchar(30) unique comment "注释",
  //非空约束(not null)，不能为空，可以重复
age int not null comment "注释",
  //MySQL对check不支持检查约束(check)
  //类检查约束(enum)枚举
/*gender varchar(10) check(gender='男' or gender='女')*/
gender enum('男','女')	//MySQL使用此条
  //外键约束 需要引擎为INNODB
foreign key(本表字段) references 另一张表(另一张表字段)
);
//修改表名
alter table 旧表名 rename to 新表名;
//修改字段
alter table 表名 change 旧字段名 新字段名 varchar(50);
//修改字段类型
alter table 表名 change 字段名 字段名 类型值;
//添加字段
alter table 表名 add 字段名 类型值;
//删除字段
alter table 表名 drop 字段名;
//新增数据
insert into 表名 values (字段1，字段2...);
//新增多条数据
insert into 表名 values (字段1,字段2...),(字段1,字段2...);
//新增指定字段数据，未指定字段默认为null
insert into 表名(字段1,字段2...) values(字段1,字段2...),(字段1,字段2...);
```
## 查询操作
```mysql
//查询表
select * from 表名;
//指定列查询
select 字段1,字段2 from 表名;
//查询表多加一列age，key都为12
select 字段1,字段2,12 age from 表名;
//查询结果给列起别名
select 字段1 别名1,字段2 别名2 from 表名;
//查询表内数据总行数--count
select count(*) 别名 from 表名;
//查询总和--sum
select sum(字段1) 别名 from 表名;
//查询平均数--avg
select avg(字段1) 别名1,avg(字段2) 别名1 from 表名;
//查询最大值
select max(字段1) as 别名1 from 表名;
//查询最小值
select min(字段1) as 别名1 from 表名;
//对null值进行处理：ifnull 对某列的空值进行处理，信息不为null，正常显示，繁殖为替换字符显示
select 字段1,ifnull(可能为空的字段,替换字符) from 表名;
//查看表结构信息
show columns from 表名;
show full columns from 表名;
//操作符	>,<,>=,<=,=,!=
select * from 表名 where 字段1>指定值;
select * from 表名 where 字段1>指定值 and 字段2>=指定值...;	范围选择
select * from 表名 where 字段1>指定值 and 字段1>=指定值...;	相同字段范围选择
select * from 表名 where 字段1>指定值 or 字段2>=指定值...;	或值选择
//in/between	
select *from 表名 where 字段1 in(指定值1,指定值2,指定值3...);	简化或值选择
select *from 表名 where 字段1 not in(指定值1,指定值2,指定值3...);	查询字段除指定值的信息
select * from 表名 where 字段1 between 指定值1 and 指定值2;	范围查询
select * from 表名 where 字段1 not between 指定值1 and 指定值2;	反向范围查询
//[NOT]null（空值判断）
select * from 表名 where 字段1 is null;
select * from 表名 where 字段1 not is null;
//模糊查询 LINK，NOT LINK(字符串匹配) 及%（0-多个字符）和_（必须有一个字符）通配符
select * from 表名 where 字段1 like '指定值%';
select * from 表名 where 字段1 like '指定值_';
select * from 表名 where 字段1 not like '%指定值%';	查询名字没有指定值的数据

//链表查询:内连接(inner join)（只能查询满足条件的值） 在一个查询结果里面，既要看到表1字段也要看到表二字段
select 表别名1.字段,表别名2.字段 from 表名1 表别名1,表名2 表别名2 where 表别名1.字段=表别名2.字段;
eg：select a.name,b.hobby from student a,hobby b where a.id=b.sid;
select 表别名1.字段,表别名2.字段 from 表名1 表别名1 inner join 表名2 表别名2 on 表别名1.字段=表别名2.字段;
//左外连接：(left outer join)，返回满足条件的数据，同时也返回左表有而右表没有的信息
select 表别名1.字段,表别名2.字段 from 表名1 表别名1 left outer join 表名2 表别名2 on 表别名1.字段=表别名2.字段 where 表别名.字段=指定值;
//右外链接：(right outer join)，返回满足条件的数据，同时也返回右表有而左表没有的信息
select 表别名1.字段,表别名2.字段 from 表名1 表别名1 right outer join 表名2 表别名2 on 表别名1.字段=表别名2.字段;
//全外链接：(full outer join / union)，返回满足条件的数据，同时也返回左表有而右表没有的信息，也返回右表有而左表没有的信息--MySQL不支持全外链接的(full outer join)写法 Oracle支持的，解决办法：
select 表别名1.字段,表别名2.字段 from 表名1 表别名1 left outer join 表名2 表别名2 on 表别名1.字段=表别名2.字段 union select 表别名1.字段,表别名2.字段 from 表名1 表别名1 right outer join 表名2 表别名2 on 表别名1.字段=表别名2.字段;
//分组查询	(group by)分组
select 字段,count(*) from 表名 group by 字段;
//分组之后对分组信息加条件 (having)必须和(group by)结合使用
select 字段,count(*) 别名 from 表名 group by 字段 having  字段>=指定值;
eg：select sid,count(*) num from hobby group by sid having num>=3;
//排序	(order by)：asc升序（默认）,desc降序
select * from 表名 order by 字段1,字段2 asc;	//先按字段1排序后再按字段2排序
select * from 表名 order by 字段 desc;
//限制查询	limit number(从哪条开始),number(查询多少条)
select * from 表名 limit number,number;
//查询结果去除重复数据	distinct
select distinct 字段1,字段2 from 表名;
//查看建表语句
show create table 表名;
// 子查询(EXISTS / NOT EXISTS)查询效率高
select 字段1,字段2 from 表名1 where exists(select 字段 from 表名 where 字段=表名1.字段);
//round 保留小数
select round(avg(字段1),number) from 表名;		number--保留小数位数
//some 查询表中数值大于查询子表中的最小值
select * from 表名 where 字段1>some(select 字段1 from 表名 where 字段2=指定值);
//any 查询表中数值大于查询子表中的任一值
select * from 表名 where 字段1>any(select 字段1 from 表名 where 字段2=指定值);
//all 查询表中数值大于查询子表中的最大值
select * from 表名 where 字段1>all(select 字段1 from 表名 where 字段2=指定值);
//并集(union/union all)去除重复的内容/不去除重复的内容
//交集(intersect)返回都存在的记录，mysql不支持
//差集(minus/excep)mysql不支持差集写法，返回左边结果集合中已有记录。而右边结果集合中没有的记录
select 字段 from 表名1 union select 字段 from 表名2;	并集
select 字段 from 表名1 union all select 字段 from 表名2;
select 字段 from 表名1 where 字段 in(select 字段 from 表名2)	类交集
select 字段 from 表名1 where 字段 not in (select 字段 from 表名2);	类差集
```
## 更新操作
```mysql
//数据更新	不加条件会将所有行的数据更新，谨记
update 表名 set 字段1=修改值,字段2=修改值 where 字段=指定值;
```
## 删除操作
```mysql
//删除信息	不加条件会将所有行的数据删除，谨记
delete from 表名 where 字段=指定值;
//删除表
drop table 表名;
```
## 索引查询
```mysql
//创建索引
create index 索引名称 on 表名(字段);
//查看索引
show index from 表名;
//删除表结构
alter table 表名 drop index 索引名称;
```
## 视图操作
```mysql
//不支持带子查询的语句，可以将子语句转化为中间视图
//创建视图
create or replace view 视图名 as mysql语句;
```
## 函数部分
```mysql
// last_day 日期的最后一天
select * from 表名 where 日期字段=last_day(日期字段);
// now 获取当前系统日期加时间，current_date 获取系统日期，year 从日期中获取年份，mounth 从日期中获取月份
select * from 表名 where year(日期字段)<year(current_date)-35;
// lower 将大写字母转为小写，upper 将小写字母转为大写字母
select lower(字段) from 表名;
select upper(字段) from 表名;
// substr(字符串,开始位置,截取长度) 截取字符串
// length(字符串) 获取指定字符串的长度
// concat(字符串1，字符串2) 拼接字符串
// replace(字符串,原始字符,替换字符)
```
## 数据类型
* 整型
>int：4
>bigint：8

* 浮点型
>float
>double

* 字符型
>char
>varchar(n)	n的范围 1-65535
>text：文本数据
>blob：大数据类型

* 日期类型
>date：（2020-03-26)
>datetime：（2020-03-26 16:50:30）

* 注意事项：
>多对多的实体机，需要引用第三张关系表
>第一范式：表中没有多值字段，表中不能有多值属性和组合属性，数据库表的每一列都是不可分割的基本数据
>第二范式：为了保证数据的唯一性，需要给表增加一个主键列，并且不存在非关键字段对任一候选关键字段的部分函数依赖（表中的非主键列，必须全部依赖表中的全部主键列）
>第三范式：没有非关键字段传递依赖主键，一张表存储另一张信息时，只能存储另一张表的主键信息，不能存储非主键信息
