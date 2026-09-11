
---

# 操作数据库
#### 1.查询所有数据库
```sql
show databases;
```
#### 2.查询当前数据库
```sql
select database();
```
#### 3.创建
```sql
create database [if not exists] 数据库名 [default charset 字符集] [collate 排序规则];
```

> [!NOTE] Title
> if not exists :如果不存在就创建
> default charset:指定字符集
> collate :

#### 4.删除
```sql
drop database [if exists] 数据库名;
```

#### 5.使用
```sql
use 数据库名;
```
---
# 操作表

## ==增==
#### 表创建
```sql
create table 表名(
	字段1 类型 [comment 注释],
	字段2 类型 [comment 注释],
	...
	字段n 类型 [comment 注释]
) [comment 表注释]
```

## ==删==
#### 删除字段
```sql
alter table 表名 drop 字段名;
```
#### 删除表
```sql
--直接删除
drop table [if exists] 表名;
--删除并重新创建该表（相当于清除数据）
truncate table 表名;
```
## ==查==
####  1.查询所有表
```sql
show tables;
```
>需要先use一个数据库
#### 2.查询表结构
```sql
desc 表名;
```
#### 3.查询指定表的建表语句
```sql
show create table 表名;
```

## ==改==
### 添加字段
```sql
alter table 表名 add 字段名 类型(长度) [comment 注释] [约束];
```

### 修改字段
```sql
--修改数据类型
alter table 表名 modify 字段名 新数据类型(长度);

--修改字段名和字段类型
alter table 表名 change 旧字段名 新字段名 类型(长度) [comment 注释] [约束];
```

#### 修改表名 
```sql
alter table 表名 rename to 新表名;
```