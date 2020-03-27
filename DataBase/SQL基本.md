# SQL 语言

## 1.详解

### from - 将所有表作笛卡尔积

```sql
Name
id name
1  a
2  b

Address
id addr
1  adda
2  addb

select * from Name, Address
id name id addr
1   a    1 adda
2   b    1 adda
1   a    2 addb
2   b    2 addb
```

### outer join (left or right join)

1. 两表笛卡尔积
2. 应用on
3. 添加外部行(以基础联结表为底进行添加)
4. 应用where

## 2.使用

### 1.注释

```sql
# 注释
SELECT *
FROM mytable; -- 注释
/* 注释1
   注释2 */
```

### 2.针对表的操作

#### 创建表

```sql
create table name
```

#### 修改表  

```sql
增加属性（列）：
alter table name add colname char(20)
删除属性：
alter table name drop column colname
```

#### 删除表  

```sql
drop table name...
```

### 3.针对内容的操作

#### 增

``` sql
insert into tablename...(cloname..., cloname...)
value(val1, val2);

insert into Person (PersonId, LastName, FirstName) values ('1', 'Wang', 'Allen')
```

#### 删

```sql
delete from tablename... 
where id = 1;// 删除一行
```

#### 改

```sql
update tablename... 
set value = 1
where id = 1
```

#### 查

``` sql
select * from tablename... where ..

# distinct 去掉重复的
select distinct value1 form tablename...

# limit n 表示查询结果返回前n条数据
limit 2,3 // 返回从index=2开始的总共3个数字

# offset n 表示跳过n条数据

# ifnull(a, b) 表示a为空则返回b

# order by xx 排序
order by colname DESC 降序
order by colname ACS 升序

# 查询第二高薪水
select ifNull(
    (select distinct Salary
     from Employee
     order by Salary desc
     limit 1 offset 1),
     null) as SecondHighestSalary // 使用 as 后面的名字作为码进行输出

# group by 将同一个类合并到一起
# having count(类别) > n
select Email
from Person
group by Email
having count(Email) > 1;
```

#### 联结

1. 左联结 left join - 结果保留左表全部数据
2. 右联结 right join - 右表全部数据
3. 内联结 inner join - 公共数据

```sql
select FirstName, LastName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId
```
