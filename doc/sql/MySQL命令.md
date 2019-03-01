<br>

<div align="center">
    <br>
    <a href="https://github.com/mayu1031/CS_Notes"> <img src="https://raw.githubusercontent.com/mayu1031/CS_Notes/master/doc/others/icons/%E5%86%AC%E6%97%A5%E8%A3%85%E5%A4%87icon/%E8%80%B3%E7%BD%A9.png"></a>
  
</div> 

<br/>


# MySQL命令

- [数据库](#数据库)
- [表格结构](#表格结构)
- [索引](#索引)
- [表格内部查询](#表格内部查询)
- [表格条件查询](#表格条件查询)


# 数据库
### **创建数据库**
```sql
CREATE DATABASE 表名;
create database abc;
```
### **显示所有的数据库**
```sql
show databases;
```
### **查看当前所在的库**
```sql
select database();
```
### **删除数据库**
```sql
DROP DATABASE 数据库名;
drop database abc;
```

# 表格结构
###  **创建指定名称的表**
```sql
create table 表名（
字段名1 字段类型（宽度） 约束条件，
字段名1 字段类型（宽度） 约束条件，
字段名1 字段类型（宽度） 约束条件，     
字段名1 字段类型（宽度） 约束条件，     
PRIMARY KEY(主键名)   );
       
create table student(
name char(5),
age int(2),
address char(20)
)
```
### **数据库中所有表格名称**
```sql
show tables;
```
### **某一表格具体字段**
```sql
desc table student;
desc student;
```
### **基本用法**
```sql
alter table 表名 执行动作；
```
### **添加新字段**
```sql
add 字段名 类型（宽度）约束条件；
alter table stu add score int(3) not null
alter table stu add phone varchar(15) not null after age;
```

### **修改字段类型**
```sql
modify 字段名 类型（宽度） 约束条件；
alter table stu modify qq int(15);
```
### **修改字段名**
```sql
change 源字段名 新字段名 类型（宽度）约束条件；
alter table stu change qq wechat varchar(20) not null;
```
### **删除指定字段**
```sql
drop 字段名；
alter table stu drop wechat
```

# 索引

### **创建索引index**
```sql
#创建表格时自带index或者create索引
create table stu（
name varchar（4） not null，
age int（3），
id char（6）not null，
course char(10) not null,
hobby char(10) not null,
index（id），index（name）
);
desc stu;

create index xxx on 表名（字段名）xxx是给索引起的名称
create index hobby course on stu（hobby); #给hobby创建了一条索引
create index index_hobby course on stu（hobby); #可以给hobby再创建一条索引，index_hobby
```

### **显示表里的索引**
```sql
show index from 表名
show index from stu; #hobby就有两条索引，一条hobby，一条index_hobby
```

### **删除索引**
```sql
drop index xxx on 表名
drop index course on stu; #把course的索引删除了
```

### **创建索引unique**
```sql
#创建表格时自带index或者create索引

create table stu（
name varchar（4） not null，
age int（3），
id char（6）not null，
course char(10) not null,
hobby char(10) not null,
unique（id），unique（name）
);
desc stu;#创建一个unique index

create unique index myCourse on stu(course); #创建一个unique index
```
### **显示表里的索引**
```sql
show unique index from 表名
show unique index from stu;
```

### **删除索引**
```sql
drop index index_hobby from stu; #s删除index
drop index hobby from stu; #删除index
```
### **创建主键 primary key**
```sql
#创建表格时自带index或者add添加主键
create table stu(
id char(6) auto increment,
...
primary key(id)
)   

alter table 表名 add primary key（id）
```
### **删除主键**
```sql
alter table 表名 drop primary key；
```

### **创建外键 foreign key**
```sql
foreign key (表A的字段名)
references （表B的字段名）
on update cascade
on delete cascade
表A的字段名参照表B的字段名

#创建AB表
create table mycourse(
course varchar(20),
course_id int(20),
primary key(course)
);

create table stu(
name char(4),
age int(2),
course varchar(2),
foreien key(course)
references mycourse(course)
on update cascade
on delete cascade
);
desc stu; (course key MUL 但是不能显示是外键)
```

### **删除外键**
```sql
alter table 表名 drop foreign key 约束名；
```

# 表格内部查询


### **插入数据**
```sql
nsert into 表名 （字段名1，字段名2，...）values (值1，值2，...)

insert into 表名 values(值1，值2，...）

insert into students values (0, 'john',18, 180.00, 1, 1, 0)；

insert into students(stu_id, name, age, height, gender, cls_id, is_delete) values (0, 'john',18, 180.00, 1, 1, 0)；

insert into students values (0, 'john',18, 180.00, 1, 1, 0),(),(),(),()；
```

### **查询语法**
```sql
select select_exper [,select_exper,...][
    from tb_name
    where 条件判断
    [group by ｛col_name| postion｝[ASC|DESC],..]
    [having where 条件判断]
    [order by ｛col_name| postion｝[ASC|DESC],..]
    [limit {}]
       
```
### **查询所有字段**
```sql
select * from 表名;   *通配符
```
### **查询指定字段**
```sql
select 列1，列2... from 表名;
```
### **用as别名给列表起名**
```sql
select id as 序列，name as 名字，gender as 性别... from 表名;
```
### **表字段名**
```sql
select students.id, student.name from students;
```
### **用as给表起别名**
```sql
select s.id, s.name, s.gender from studnets as s;
```
### **消除重复行**
```sql
select distinct name from students；
select distinct age from students；
```
- **Distinct和Group by去除重复字段记录**   
  一是完全重复的记录，也即所有字段均重复的记录  
  二是部分关键字段重复的记录，比如Name字段重复，而其他字段不一定重复或都重复可以忽略。
  
    - select count(distinct col) from A;
    - select count(1) from (select 1 from A group by col) as C;

# 表格条件查询

### **基本语法**
```sql
select * from 表名
where 条件;
```

### **比较运算符**
```sql
等于 =
大于 >
大于等于 >=
小于 <
小于等于 <=
不等于 !=/ <>

select * from students
where id <=4;

select * from students
where name != 'john';
```

### **逻辑运算符**
```sql
and
or
not

select * from student
where id>3 and gender =1;

```

### **模糊查询**
- like
```sql
%表示任意多个任意字符
_表示任意一个字符

select * from students
where name like '黄%'
```
- '_'
```sql
select * from students
where name like '黄_' #黄后面是一个字的学生

select * from students
where name like '黄__' #黄后面是两个字的学生

select * from students
where name like '黄%' or name like '梅%'
```
### **范围查询**
- in
```sql
in表示在一个非连续的范围内
select * from students
where id in (1,3,8);
```
- between
```sql
select * from students
where id between 3 and 8;
```
- 空判断
```sql
select * from students
where height is not null;
```
### **排序**
```sql
order by，默认 asc 从小到大，升序

select * from 表名
order by 列1 asc|desc, 列2 asc|desc;

select * from students
where is_delete = 0 
order by name;
```
### **聚合函数**
- count(*)
```sql
select count(*) from students;
```
- max(列)
```sql
select max(id) from students;
```
- min(列)
```sql
select min(id) from students
where is_delete=0;
```
- avg(列)
```sql
select sum(age)/count(*) from students;
select avg(age) from students;
```
- sum(列)
```sql
select sum(age) from students
where gender = '男';
```
### **分组**
- group by
- group by + group_concat 函数
```sql
select gender, group_concat(name) from students
group by gender;
```
- group by +集合函数
```sql
select gender, group_concat(age) from students
group by gender;
```
- group + having
```sql
select gender, group_concat(name) from students
group by gender
having count(*)>7; ##按性别分组，把组成员个数大于7的那一列名字列出来
```
- group + with rollup
```sql
select gender, count(*) from students
group by gender
with rollup;

-gender count(*)
-男      8
-女      5
-NULL    13

select gender, group_concat(age) from students
group by gender
with rollup;
```
### **分页获取内容**  
limit
limit offset
```sql
select *from 表名 
limit start,count;

select * from students limit 5,5
## id从6开始，显示5个
```
```sql
limit 2 offset 3
##跳过3个值，显示2个
```
### **连接查询**
```sql
select * from 表1
inner join/ left join/ right join 表2 on 表1.列 = 表2.列
```
### **自关联**

```sql
create table areas(
    aid int primary key, #area
    atitle varchar(20) #name
    pid int
)
```
### **子查询**
- 标量子查询
```sql
select * from students
where age > 
(select avg(age) from students);
```
- 列级子查询
```sql
select name from classes
where id in (select cls_id from students);
#先查班级id，在查在这个id里面的学生
```
行级子查询
```sql
select * from students 
where (height,age) = (select max(height),max(age) from students)
```

### **union, union all**
- UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
- 请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

SQL UNION 语法

```
SELECT column_name(s) FROM table_name1
UNION
SELECT column_name(s) FROM table_name2
```

- 注释：默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。
- UNION ALL 命令和 UNION 命令几乎是等效的，不过 UNION ALL 命令会列出所有的值。 

SQL UNION ALL 语法

```
SELECT column_name(s) FROM table_name1
UNION ALL
SELECT column_name(s) FROM table_name2
```
另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。
