
> [!important] 通用语法
> 不区分大小写
> 单行注释：-- xxx 或 # xxx
> 多行注释：/\*xxx\*/

---
# SQL语句分类

| 类型      | 说明                          |
| ------- | --------------------------- |
| [[DDL]] | 数据定义语言，用来定义书库库对象（数据库，表，字段）  |
| [[DML]] | 数据操作语言，用来对数据库表中的数据进行增删改     |
| [[DQL]] | 数据查询语言，用来查询数据库中表的记录         |
| [[DCL]] | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |

---
# 数据类型 
#### 1.数值类型

| 类型          |   大小    |
| :---------- | :-----: |
| tinyint     | 1 byte  |
| smallint    | 2 bytes |
| mediumint   | 3 bytes |
| int/integer | 4 bytes |
| bigint      | 8 bytes |
| float       | 4 bytes |
| double      | 8 bytes |
| decimal     |    -    |
```sql
create table test (
	# 末尾加unsigned,表示无符号
	age tinyint unsigned,
	# double 需要指定两个参数，4表示长度（100.0），1表示小数位数（75.8，一位小数）
	score double(4.1),
)
```
#### 2.字符串类型

| 类型         | 描述              |
| ---------- | --------------- |
| char       | 定长字符串           |
| varchar    | 变长字符串           |
| tinyblob   | 不超过255个字符的二进制数据 |
| tinytext   | 短文本字符串          |
| blob       | 二进制形式的长文本字符串    |
| text       | 长文本数据           |
| mediumblob | 二进制形式的中等长度文本数据  |
| mediumtext | 中等长度文本数据        |
| longblob   | 二进制形式的极大文本数据    |
| longtext   | 极大文本数据          |
```sql 
create table test(
	# 创建时需要指定长度
	name_1 char(10),
	name_2 varchar(10)
)
```
> [!info] char和varchar区别
> 1.char为定长，不论数据是否占满空间
> 2.varchar会根据数据大小，在不超过指定长度的情况下动态调整（需要多少用多少）
> 3.char性能更好


#### 3.日期时间类

| 类型        | 格式                  | 描述           |
| --------- | ------------------- | ------------ |
| date      | YYYY-MM-DD          | 日期值          |
| time      | HH:MM:SS            | 时间值或持续时间     |
| year      | YYYY                | 年份值          |
| datetime  | YYYY-MM-DD HH:MM:SS | 混合日期和时间值     |
| timestamp | YYYY-MM-DD HH:MM:SS | 混合日期和时间值，时间戳 |
