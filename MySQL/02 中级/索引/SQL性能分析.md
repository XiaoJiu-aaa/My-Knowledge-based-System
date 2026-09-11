## 1.SQL执行频率
```sql
-- 查看insert、update、delete、select的频次
show [session | global] status like 'Com_'
```
## 2.慢查询日志

> [!NOTE] 定义
> 慢查询日志记录了所有执行时间超过指定参数（long_query_time 默认10秒）的所有SQL语句的日志
```sql
-- 查看开关是否打开
show variables like 'slow_query_log';

-- 进入MySQL的配置文件：/etc/my.cnf
--开启
slow_query_log = 1
-- 设置时间
long_query_tiem = 2
```

> 默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用参数来开启
```etc/my.cnf
# 记录执行较慢的管理语句
log_slow_admin_statements = 1
# 记录执行较慢的未使用索引的语句
log_queries_not_using_indexs = 1
```
## 3.profile详情

> [!NOTE] 说明
> ==show profiles==能够在做SQL优化时帮助了解时间都耗费到哪里去了。通过have_profiling参数，能够看到当前MySQL是否支持profile操作

```sql
select @@have_profiling;
-- 默认关闭,可以手动开启
set [session | global] profiling = 1;

-- 查看每一条SQL的耗时情况
show profiles;

-- 查看指定query_id的SQL语句在各个阶段的耗时情况
show profile for query query_id;

-- 查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id;
```

## 4.explain执行计划

> [!NOTE] 说明
> explain或desc命令获取MySQL如何执行select语句信息，包括在select语句执行过程中表如何连接和连接的顺序

```sql
explain select ...;
```

##### explain执行计划各字段的含义
###### 1.id
	select查询的序列号，id 越大越先执行；id 相同则从上往下；id 为 NULL 最后执行（操作临时表）
###### 2.select_type
	表示select的类型，常见取值有simple(简单表，即不适用表连接或者子查询)、primary(主查询，即外层查询)、unique(union中的第二个或者后面的查询语句)、subquery(select/where之后包含了子查询)等
###### 3.==type==
	表示连接类型，性能由好到差的连接类型为:
		NULL、system、const、eq_ref、ref、range、index、all
##### ==4.possible_key==
	显示可能应用在这张表上的索引，一个或多个
###### 5.==Key==
	实际使用的索引，如果为NULL，则没有使用索引
###### 6。==Key_len==
	表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好
###### 7.rows
	MySQL认为必须要执行查询的行数，在innoDB引擎的表中，是一个估计值
###### 8. filtered
	表示返回结果的行数占需读取行数的百分比，filtered越大越好
###### 9. Extra
	额外的信息