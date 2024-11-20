---
title: "Ubuntu安装PostgreSQL的教程"
date: 2024-11-20T10:17:23+08:00
draft: false
---

## 安装

使用包管理器就可以直接安装PostgreSQL：
```sh
sudo apt install postgresql
```

## 配置

### 创建新用户和数据库

默认情况下，PostgreSQL会创建一个名为`postgres`的数据库超级用户以及一个同名的Unix用户（注意区分数据库用户和Unix用户的概念）。`postgres`数据库用户没有密码，只有在主机上登入`postgres` Unix用户之后，才有权限登入`postgres`数据库用户。`psql`是PostgreSQL的客户端命令行工具。输入下面的命令就可以通过`postgres`数据库用户来操纵数据库。
```sh
sudo -u postgres psql
```

然后输入以下命令，就可以创建一个名为`user1`的用户和名为`db1`的数据库，并赋予`user1`操纵`db1`的所有权限。
```sql
CREATE USER user1 WITH PASSWORD 'your_password';
CREATE DATABASE db1;
GRANT ALL PRIVILEGES ON DATABASE db1 TO user1;
\c db1 postgres
GRANT ALL ON SCHEMA public TO user1;
\q
```

顺带一提，可以通过`pwgen`命令生成一个随机的16位强密码：
```sh
sudo apt install pwgen
pwgen -s 16 1
```

### 允许外网通过密码连接

默认配置下，PostgreSQL不允许外网连接通过密码登入数据库。下面介绍如何解除该限制。

首先编辑`/etc/postgresql/*/main/postgresql.conf`（这里的`*`代表PostgreSQL的版本号，下同），加入这一行：
```
listen_addresses = '*'
```

然后在`/etc/postgresql/*/main/pg_hba.conf`中加入这一行：
```
host  all  all  0.0.0.0/0  scram-sha-256
```

你可以通过下面的命令快速编辑配置文件：
```sh
echo "listen_addresses = '*'" | sudo tee -a /etc/postgresql/*/main/postgresql.conf
echo "host  all  all  0.0.0.0/0  scram-sha-256" | sudo tee -a /etc/postgresql/*/main/pg_hba.conf
```

### 将数据库文件移动至数据盘中

默认情况下，PostgreSQL的数据库文件存储在`/var/lib/postgresql/*/main/`下面。可以通过软链接将数据库文件迁移到别的目录。此外要注意定期备份。

## 启动

输入以下命令即可启动PostgreSQL，并设置开机自动启动：
```sh
sudo systemctl start postgresql
sudo systemctl enable postgresql
```
