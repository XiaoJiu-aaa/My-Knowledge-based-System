# 部署
```bash
# 配置仓库
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022 
# 5.x仓库
rpm -Uvh http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm
# 8.x仓库
rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm
# 下载
yum -y install mysql-community-server

# 获取初始密码
grep 'temporary password' /var/log/mysqld.log
```
```sql
-- 更改密码
alter user 'root'@'localhost' identified by 'xxxx';
--Jzx123456mysql!
-- 配置root的远程登录5.0 8.0
grant all privileges on *.* to root@'%' identified by 'Jzx123456mysql!' with grant option;
create user 'root'@'%' identified with mysql_native_password by 'Jzx123456mysql!';
--刷新
flush privileges;

-- 退出登录
exit;
```