# SQL 语言

## 注释

```sql
# 注释
SELECT *
FROM mytable; -- 注释
/* 注释1
   注释2 */
```

## 针对表的操作

### 创建表

```sql
create table name...
```

### 修改表  

```sql
增加属性（列）：
alter table name... add colname... char(20) 
删除属性：
alter table name... drop column colname... 
```

### 删除表  

```sql
drop table name...
```

## 针对内容的操作

### 增

``` sql
insert into tablename...(cloname..., cloname...)
value(val1, val2);
```

### 删

```sql
delete from tablename... 
where id = 1;// 删除一行
```

### 改

```sql
update tablename... 
set value = 1
where id = 1
```

### 查

``` sql
select * from tablename... where ..
select distinct value1 form tablename... // 去掉重复的
select vaule1 form tablename... limit 2,3 // 返回从index=2开始的总共3个数字
```
