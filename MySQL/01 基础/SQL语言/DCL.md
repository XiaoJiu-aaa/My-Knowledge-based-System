# 1.查询用户
```sql
use mysql;
select * from user;
```

# 2.创建用户
```sql
alter user '用户名'@'主机名' identified by '密码';
```

# 3.修改用户密码
```sql
alter user '用户名'@'主机名' identified with mysql_native_password '新密码';
```
# 4.删除用户
```sql
drop user '用户名'@'主机名';
-- 主机名可以用%通配符
```
# 5.权限控制
 

| 权限                 | 说明         |
| ------------------ | ---------- |
| all,all privileges | 所有权限       |
| select             | 查询数据       |
| insert             | 插入数据       |
| update             | 修改数据       |
| delete             | 删除数据       |
| alter              | 修改表        |
| drop               | 删除数据库/表/视图 |
| create             | 创建数据库/表    |
#### a.查询权限
```sql
show grants for '用户名'@'主机名';
```
#### b.授予权限
```sql
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
```
#### c.撤销权限
```sql
revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
```