title: Mysql命令
author: Asia Cui
tags:
  - MySQL
categories:
  - MySQL命令
date: 2019-01-09 11:54:00
---
> 此文针对MySQL5.7版本，5.7以上版本可能有不支持情况。

## MySQL命令



```bash
#控制台登录mysql的root用户，linux如果执行命令不存在，未把mysql添加到bin目录下
mysql -uroot -p

#选择操作的数据库名称
use mysql;
#查看所有的库
show database;
#查看所有的表，需要在选择库之后才能使用
show tables;

#执行脚本文件
source /usr/test.sql

#退出mysql控制台
exit;

#查看行锁的竞争状态
show status like 'innodb_row_lock%';
```

## MySQL配置

- 修改密码

  ```bash
  #知道密码情况下，修改密码
  use mysql;
  update user set password=passworD("123456") where user='root';
  
  #忘记密码情况下，修改密码
  首先，你必须要有操作系统的root权限了，找到my.conf编辑，把skip-grant-tables粘贴到[mysqld]选项中
  ```

- mysql创建用户

  ```bash
  #创建用户
  use mysql;
  CREATE USER 'username'@'host' IDENTIFIED BY 'password';
  - username：创建的用户名
  - host：指定该用户在哪个主机上可以登陆，为"localhost"指该用户只能在本地登录，将"localhost"改为"%"，表示在任何一台电脑上都可以登录;也可以指定某台机器可以远程登录;
  - password:用户的密码。
  
  #例：
  CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
  ```

## CURD

```sql
#查询语句
select * from sys_user where id = 1;
#更新语句
update sys_user set name='admin' where id = 1;
```
## 索引

```sql
#给表A的name字段添加唯一索引
alter table A add unique(name);
```
