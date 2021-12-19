---
title: mariadb-install
date: 2021-09-18 15:03:55
tags: database
---
# env
centos 8.2

# base step

1. 服务包
```sh
yum install mariadb-server
```

2. 设置服务

```sh
systemctl start mariadb
systemctl enable mariadb
```

3. 安全设置

```sh
mysql_secure_installation
```

# 设置字符集为utf-8

在指定文件添加或修改以下内容
1. /etc/my.cnf 
- [mysqld] 

```sh
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
```
2. /etc/my.cnf.d/client.cnf 
- [client]
```sh
default-character-set=utf8
```
- [mysql]
```sh
default-character-set=utf8
```

3. 重启mariadb-server
```sh
systemctl restart mariadb
```

# 允许远端登录

1. sql数据库更改允许host

```mysql
select host, user from user;
update user set host='%' where host='mini';
```

2.重启mariadb

```sh
systemctl restart mariadb
```
