
# 1.系统数据库

| 数据库                | 含义                                                 |
| ------------------ | -------------------------------------------------- |
| mysql              | 存储MySQL服务器正常运行所需要的各种信息（时区、主从、用户、权限等）               |
| information_schema | 提供了访问数据库元数据的各种表和视图，包含数据库、表、字段类型及访问权限等              |
| performance_schema | 为MySQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数         |
| sys                | 包含一系列方便DBA和开发人员利用performance_schema性能数据库进行调优和诊断的视图 |
# 2.常用工具
#### mysql
```shell
# 语法
mysql [options] [database]
# 选项
	-u,--user=name       # 指定用户名
	-p,--password[=name] # 指定密码
	-h,--host=name       # 指定服务器IP或域名
	-P,--port=port       # 指定连接端口
	-e,--excute=name     # 执行SQL语句并退出
```
#### mysqladmin
>是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和当前状态，创建并删除数据库等。
```shell
mysqladmin --help
```
#### mysqlbinlog
>由于服务器生成的二进制日志文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog日志管理器工具
```shell
mysqlbinlog [参数选项] logfilename
```
	-d : 指定数据库名称，只列出指定的数据库相关操作
	-o : 忽略掉日志中的前n行命令
	-v : 将行事件（数据变更）重构为SQL语句
	-w : 将行事件（数据变更）重构为SQL语句，并输出注释信息
#### mysqlshow
> 客户端查找工具，用来很快的查找存在哪些数据库、数据库中的表、表中的列或索引
```shell
mysqlshow
```
#### mysqldump
> 用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句
```shell
mysqldump [options] db_name[tables]

```
#### mysqlimport/source
> mysqlimport客户端数据导入工具，用来导入mysqldump加-T参数后导出的文本文件
> 如果需要导入sql文件，可以使用mysql中的source命令
```shell
mysqlimport [options] db_name textfile1 textfile2...
```