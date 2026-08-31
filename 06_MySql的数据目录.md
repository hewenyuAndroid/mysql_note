[toc]

# 1、在 `Docker` 中创建 `mysql` 容器

## 1.1、创建 `mysql 5.7` 版本容器

在 `docker` 中启动 MySQL 5.7 镜像的容器

```shell
# docker run --name mysql5.7 -dp 3330:3306 -v /home/hewenyu/docker/mysql5.7/data:/var/lib/mysql -v /home/hewenyu/docker/mysql5.7/log:/var/log/mysql -v /home/hewenyu/docker/mysql5.7/config:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
# 启动 mysql5.7 版本镜像的容器
# 宿主机端口 3330 映射到mysql5.7 容器的 3306 端口
# 宿主机 /home/hewenyu/docker/mysql5.7/data 目录映射容器 /var/lib/mysql 目录
# 宿主机 /home/hewenyu/docker/mysql5.7/log 目录映射容器 /var/log/mysql 目录
# 宿主机 /home/hewenyu/docker/mysql5.7/config 目录映射 /etc/mysql/conf.d 目录
# 初始密码是 123456

hewenyu@hewenyu:/mnt/c/Users/he875$ docker run --name mysql5.7 -dp 3330:3306 -v /home/hewenyu/docker/mysql5.7/data:/var/
lib/mysql -v /home/hewenyu/docker/mysql5.7/log:/var/log/mysql -v /home/hewenyu/docker/mysql5.7/config:/etc/mysql/conf.d
-e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
0cbb1d9759f1bcf695bc05682ef9e9cca255c4c50af1868c8015e56b5896e589
hewenyu@hewenyu:/mnt/c/Users/he875$ docker ps | grep mysql5.7
0cbb1d9759f1   mysql:5.7   "docker-entrypoint.s…"   10 seconds ago   Up 9 seconds    33060/tcp, 0.0.0.0:3330->3306/tcp, [::]:3330->3306/tcp   mysql5.7
hewenyu@hewenyu:/mnt/c/Users/he875$ docker exec -it mysql5.7 /bin/bash
bash-4.2# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.44 MySQL Community Server (GPL)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select version();
+-----------+
| version() |
+-----------+
| 5.7.44    |
+-----------+
1 row in set (0.00 sec)

mysql>
```

## 1.2、创建 `mysql 8.4` 版本容器

在 `docker` 中启动 `mysql 8.4` 版本镜像容器

```shell
# docker run --name mysql8.4 -dp 3332:3306 -v /home/hewenyu/docker/mysql8.4/data:/var/lib/mysql -v /home/hewenyu/docker/mysql8.4/log:/var/log/mysql -v /home/hewenyu/docker/mysql8.4/config:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 mysql:8.4

hewenyu@hewenyu:/mnt/c/Users/he875$ docker run --name mysql8.4 -dp 3332:3306 -v /home/hewenyu/docker/mysql8.4/data:/var/lib/mysql -v /home/hewenyu/docker/mysql8.4/log:/var/log/mysql -v /home/hewenyu/docker/mysql8.4/config:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=123456 mysql:8.4
b8c2278d7de4e2d098900a8179187838df828727ba0c3453285f0dcf3cc63cf4
hewenyu@hewenyu:/mnt/c/Users/he875$ docker ps | grep mysql8.4
b8c2278d7de4   mysql:8.4   "docker-entrypoint.s…"   14 seconds ago   Up 14 seconds   33060/tcp, 0.0.0.0:3332->3306/tcp, [::]:3332->3306/tcp   mysql8.4
hewenyu@hewenyu:/mnt/c/Users/he875$ docker exec -it mysql8.4 /bin/bash
bash-5.1# mysql -uroot -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.4.11 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select version();
+-----------+
| version() |
+-----------+
| 8.4.11    |
+-----------+
1 row in set (0.00 sec)

mysql>
```

# 2、`Mysql` 的数据目录

> mysql 5.7

```shell
mysql> show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.02 sec)
```

> mysql 8.4

```shell
mysql> show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.01 sec)
```

`MySql` 的所有用户数据、系统元素据、事务日志等物理存储在 `/var/lib/mysql/` 数据目录下。一旦 `datadir` 损坏或丢失，整个`MySql`服务的数据将不可恢复（除非有备份）。

默认情况下，`MySql` 的 `datadir` 目录都指向 `/var/lib/mysql/`，但是不同版本之间 `datadir` 里面存储的数据文件不同。

# 3、`MySql` 的命令目录

`/usr/bin` 是 `Linux` 系统中存放普通用户可执行的系统命令和应用程序的核心目录。

> mysql 5.7

```shell
bash-4.2# pwd
/usr/bin
bash-4.2# ls -la | grep mysql
-rwxr-xr-x 1 root root  4204288 Oct 11  2023 mysql
-rwxr-xr-x 1 root root  8108264 Oct 14  2023 mysql-secret-store-login-path
-rwxr-xr-x 1 root root     5197 Oct 11  2023 mysql_config
-rwxr-xr-x 1 root root  5257000 Oct 11  2023 mysql_install_db
-rwxr-xr-x 1 root root  3652064 Oct 11  2023 mysql_ssl_rsa_setup
-rwxr-xr-x 1 root root  3561616 Oct 11  2023 mysql_tzinfo_to_sql
-rwxr-xr-x 1 root root  4534056 Oct 11  2023 mysql_upgrade
-rwxr-xr-x 1 root root  3878680 Oct 11  2023 mysqladmin
-rwxr-xr-x 1 root root  3963136 Oct 11  2023 mysqldump
-rwxr-xr-x 1 root root  4456760 Oct 11  2023 mysqlpump
-rwxr-xr-x 1 root root 40283008 Oct 14  2023 mysqlsh
bash-4.2#
```

> mysql 8.4

```shell
bash-5.1# pwd
/usr/bin
bash-5.1# ls -la | grep mysql
-rwxr-xr-x 1 root root  7754312 Jun 30 19:24 mysql
-rwxr-xr-x 1 root root  8016512 Jun 10 13:55 mysql-secret-store-login-path
-rwxr-xr-x 1 root root     5053 Jun 30 18:43 mysql_config
-rwxr-xr-x 1 root root  7483992 Jun 30 19:24 mysql_migrate_keyring
-rwxr-xr-x 1 root root   150792 Jun 30 19:24 mysql_tzinfo_to_sql
-rwxr-xr-x 1 root root  7421192 Jun 30 19:24 mysqladmin
-rwxr-xr-x 1 root root  7508264 Jun 30 19:24 mysqldump
-rwxr-xr-x 1 root root 27720912 Jun 10 13:55 mysqlsh
bash-5.1#
```

- `mysqladmin`: 用于执行一些不需要交互登录的操作。常见用法包括:
  - 检查服务器是否存活：`mysqladmin ping`
  - 关闭数据库：`mysqladmin shutdown`
  - 创建或删除数据库：`mysqladmin create dbname / drop dbname`
  - 刷新权限或日志：`mysqladmin flush-privileges / flush-logs`
- `mysqldump`：这是最常用的逻辑备份工具。它会将数据库中的数据导出为一系列 `SQL` 语句（如 `CREATE TABLE`、`INSERT INTO`），保存为一个文本文件。可以用这个文件来重建整个数据库或特定表，非常适合做数据迁移或定期备份。


```shell
# 运行指令需要带上账号密码
bash-5.1# mysqladmin -uroot -p ping
Enter password:
mysqld is alive
bash-5.1#
```

# 4、`MySql` 的配置目录

## 4.1、`my.cnf` 全局配置文件

`MySql` 在启动时会按以下顺序查找 `my.cnf` 配置文件（越靠前优先级越高）：

1. `/etc/my.cnf​` （全局配置文件，最常用）
2. `/etc/mysql/my.cnf​` （Debian/Ubuntu 系常用）
3. `~/.my.cnf`​ （用户级，仅影响当前用户连接的客户端）

`MySql` 按照顺序找到上述任意一个 `my.cnf` 配置文件，就会停止搜索 `my.cnf` 配置文件，后续的 `my.cnf` 文件将不会被 `MySql` 解析到;

> mysql 5.7  `/etc/my.cnf`

```shell
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
skip-host-cache
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=/var/lib/mysql-files
user=mysql

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

#log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
[client]
socket=/var/run/mysqld/mysqld.sock

# 加载 /etc/mysql/conf.d/ 目录下的所有 .cnf 配置文件到当前位置
# 同名的配置，后面的配置会覆盖前面的配置
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/
```

> mysql 8.4 `/etc/my.cnf`

```shell
# /etc/my.cnf

# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/8.4/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M

host-cache-size=0
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/run/mysqld/mysqld.sock
secure-file-priv=/var/lib/mysql-files
user=mysql

pid-file=/var/run/mysqld/mysqld.pid
[client]
socket=/var/run/mysqld/mysqld.sock

# 加载 /etc/mysql/conf.d/ 目录下的所有 .cnf 配置文件到当前位置
# 同名的配置，后面的配置会覆盖前面的配置
!includedir /etc/mysql/conf.d/
```

## 4.2、`/usr/share/mysql` 共享资源目录

`/usr/share/mysql` 是 `MySQL` 的共享资源目录，存放的是只读、架构无关的辅助文件。常见的大类是：

- 字符集文件：各种 `collation` 的定义数据
- 错误消息文件：不同语言的错误提示（如 `errmsg.sys`）
- 配置文件示例：如 `my-huge.cnf`、`my-large.cnf`、`my-medium.cnf`、`my-small.cnf` 等模板
- SQL 初始化/基准测试脚本：如 `sql-bench` 相关脚本
- 时区数据（部分版本）：配合 `mysql_tzinfo_to_sql` 使用


> mysql 5.7  `/usr/share/mysql`

```shell
bash-4.2# pwd
/usr/share/mysql

bash-4.2# ls
bulgarian             french                       mysql_security_commands.sql   russian
charsets              german                       mysql_sys_schema.sql          serbian
czech                 greek                        mysql_system_tables.sql       slovak
danish                hungarian                    mysql_system_tables_data.sql  spanish
dictionary.txt        innodb_memcached_config.sql  mysql_test_data_timezone.sql  swedish
dutch                 install_rewriter.sql         norwegian                     ukrainian
english               italian                      norwegian-ny                  uninstall_rewriter.sql
errmsg-utf8.txt       japanese                     polish
estonian              korean                       portuguese
fill_help_tables.sql  magic                        romanian
bash-4.2#
```

> mysql 8.4 `/usr/share/mysql8.4`

```shell
bash-5.1# pwd
/usr/share/mysql-8.4

bash-5.1# ls
bulgarian       dutch     greek                 korean                     polish      slovak
charsets        english   hungarian             messages_to_clients.txt    portuguese  spanish
czech           estonian  install_rewriter.sql  messages_to_error_log.txt  romanian    swedish
danish          french    italian               norwegian                  russian     ukrainian
dictionary.txt  german    japanese              norwegian-ny               serbian     uninstall_rewriter.sql
bash-5.1#
```









## 2.1、`MySql 8.4` 的数据目录



## 2.2、`MySql 5.5` 的数据目录

### 1.1.1 `mysql` 数据库文件的存放目录: `/var/lib/mysql/`

```shell
mysql> show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.00 sec)
```

### 1.1.2 查看 `mysql` 相关命令目录 `/usr/bin`

`/usr/bin` 是 `Linux` 系统中存放普通用户可执行的系统命令和应用程序的核心目录。

```shell
bash-5.1# pwd
# mysql 命令目录
/usr/bin
bash-5.1# ls -la | grep mysql
-rwxr-xr-x 1 root root  7754312 Jun 30 19:24 mysql
-rwxr-xr-x 1 root root  8016512 Jun 10 13:55 mysql-secret-store-login-path
-rwxr-xr-x 1 root root     5053 Jun 30 18:43 mysql_config
-rwxr-xr-x 1 root root  7483992 Jun 30 19:24 mysql_migrate_keyring
-rwxr-xr-x 1 root root   150792 Jun 30 19:24 mysql_tzinfo_to_sql
-rwxr-xr-x 1 root root  7421192 Jun 30 19:24 mysqladmin
-rwxr-xr-x 1 root root  7508264 Jun 30 19:24 mysqldump
-rwxr-xr-x 1 root root 27720912 Jun 10 13:55 mysqlsh
```

- `mysqladmin`: 用于执行一些不需要交互登录的操作。常见用法包括:
  - 检查服务器是否存活：`mysqladmin ping`
  - 关闭数据库：`mysqladmin shutdown`
  - 创建或删除数据库：`mysqladmin create dbname / drop dbname`
  - 刷新权限或日志：`mysqladmin flush-privileges / flush-logs`
- `mysqldump`：这是最常用的逻辑备份工具。它会将数据库中的数据导出为一系列 `SQL` 语句（如 `CREATE TABLE`、`INSERT INTO`），保存为一个文本文件。可以用这个文件来重建整个数据库或特定表，非常适合做数据迁移或定期备份。
- `mysqlbinlog`：二进制日志解析工具，`MySQL` 会记录所有修改数据的操作到二进制日志文件中（`binlog`），用于数据恢复和主从复制。

### 1.1.3 配置文件目录

- `/usr/share/mysql-8.4` 命令及配置文件
- `/etc/mysql/` (`my.cnf`)

```shell
bash-5.1# pwd
/usr/share/mysql-8.4
bash-5.1# ls
bulgarian       dutch     greek                 korean                     polish      slovak
charsets        english   hungarian             messages_to_clients.txt    portuguese  spanish
czech           estonian  install_rewriter.sql  messages_to_error_log.txt  romanian    swedish
danish          french    italian               norwegian                  russian     ukrainian
dictionary.txt  german    japanese              norwegian-ny               serbian     uninstall_rewriter.sql
bash-5.1#


bash-5.1# pwd
/etc/mysql
bash-5.1# ls -la
total 12
drwxr-xr-x 3 root root 4096 Jul 27 22:07 .
drwxr-xr-x 1 root root 4096 Aug 24 12:53 ..
drwxr-xr-x 2 root root 4096 Aug 24 13:07 conf.d
bash-5.1#
```

# 2、数据库和文件系统的关系

## 2.1、`MySql` 自带的数据库

```shell
# 查看当前有哪些数据库
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| testdb             |
+--------------------+
5 rows in set (0.01 sec)
```

`MySql` 中有四个数据库是属于自带的系统数据库:

## 2.1.1、 `mysql`

`MySQL` 系统自带的核心数据库，它存储了`MySQL`的用户账户和权限信息，一些存储过程、事件的定义信息，一些运行过程中产生的日志信息，一些帮助信息以及时区信息等。

### 2.1.2、 `information_schema`

`MySQL` 系统自带的数据库，这个数据库保存着 `MySQL` 服务器维护的所有其他数据库的信息，比如有哪些表、哪些视图、哪些触发器、哪些列、哪些索引。这些信息并不是真实的用户数据，而是一些描述性信息，有时候也称之为元数据。在系统数据库 `information_schema` 中提供了一些以 `innodb_sys` 开头的表，用于表示内部系统表。

```shell
mysql> show tables like 'innodb%';
+----------------------------------------+
| Tables_in_information_schema (INNODB%) |
+----------------------------------------+
| INNODB_BUFFER_PAGE                     |
| INNODB_BUFFER_PAGE_LRU                 |
| INNODB_BUFFER_POOL_STATS               |
| INNODB_CACHED_INDEXES                  |
| INNODB_CMP                             |
| INNODB_CMPMEM                          |
| INNODB_CMPMEM_RESET                    |
| INNODB_CMP_PER_INDEX                   |
| INNODB_CMP_PER_INDEX_RESET             |
| INNODB_CMP_RESET                       |
| INNODB_COLUMNS                         |
| INNODB_DATAFILES                       |
| INNODB_FIELDS                          |
| INNODB_FOREIGN                         |
| INNODB_FOREIGN_COLS                    |
| INNODB_FT_BEING_DELETED                |
| INNODB_FT_CONFIG                       |
| INNODB_FT_DEFAULT_STOPWORD             |
| INNODB_FT_DELETED                      |
| INNODB_FT_INDEX_CACHE                  |
| INNODB_FT_INDEX_TABLE                  |
| INNODB_INDEXES                         |
| INNODB_METRICS                         |
| INNODB_SESSION_TEMP_TABLESPACES        |
| INNODB_TABLES                          |
| INNODB_TABLESPACES                     |
| INNODB_TABLESPACES_BRIEF               |
| INNODB_TABLESTATS                      |
| INNODB_TEMP_TABLE_INFO                 |
| INNODB_TRX                             |
| INNODB_VIRTUAL                         |
+----------------------------------------+
31 rows in set (0.00 sec)

mysql>
```

注意，在 `MySQL 5.7` 及更早版本中，`information_schema` 里的 `InnoDB` 系统表名称以 `INNODB_SYS_` 开头，例如：

```shell
INNODB_SYS_TABLES
INNODB_SYS_COLUMNS
INNODB_SYS_INDEXES
```

从 `MySQL 8.0` 开始，这些表被统一简化为 `INNODB_` 前缀，去掉了中间的 `SYS`，变为：

```shell
INNODB_TABLES
INNODB_COLUMNS
INNODB_INDEXES
```

### 2.1.3、 `performance_schema`

`MySQL` 系统自带的数据库，这个数据库里主要保存 `MySQL` 服务器运行过程中的一些状态信息，可以用来监控 `MySQL` 服务的各类性能指标。包括统计最近执行了哪些语句，在执行过程的每个阶段都花费了多长时间，内存的使用情况等信息。

### 2.1.4 `sys`

`MySQL` 系统自带的数据库，这个数据库主要是通过视图的形式把 `information_schema` 和 `performance_schema` 结合起来，帮助系统管理员和开发人员监控 `MySQL` 的技术性能。

## 2.2、数据库在文件系统中的表示

查看计算机上的 `/var/lib/mysql` 目录

```shell
bash-5.1# pwd
/var/lib/mysql
bash-5.1# ls -la
total 108516
-rw-r----- 1 mysql mysql  4194304 Aug 30 07:11 '#ib_16384_0.dblwr'
-rw-r----- 1 mysql mysql 12582912 Aug 24 12:30 '#ib_16384_1.dblwr'
drwxr-x--- 2 mysql mysql     4096 Aug 30 07:09 '#innodb_redo'
drwxr-x--- 2 mysql mysql     4096 Aug 30 07:09 '#innodb_temp'
drwxr-xr-x 8 mysql root      4096 Aug 30 07:09  .
drwxr-xr-x 1 root  root      4096 Jul 27 22:07  ..
-rw-r----- 1 mysql mysql       56 Aug 24 12:30  auto.cnf
-rw-r----- 1 mysql mysql  2996555 Aug 24 12:30  binlog.000001
-rw-r----- 1 mysql mysql      917 Aug 24 12:48  binlog.000002
-rw-r----- 1 mysql mysql      181 Aug 24 13:01  binlog.000003
-rw-r----- 1 mysql mysql       48 Aug 24 12:53  binlog.index
-rw------- 1 mysql mysql     1705 Aug 24 12:30  ca-key.pem
-rw-r--r-- 1 mysql mysql     1112 Aug 24 12:30  ca.pem
-rw-r--r-- 1 mysql mysql     1112 Aug 24 12:30  client-cert.pem
-rw------- 1 mysql mysql     1705 Aug 24 12:30  client-key.pem
-rw-r----- 1 mysql mysql     3598 Aug 26 08:11  ib_buffer_pool
-rw-r----- 1 mysql mysql 12582912 Aug 30 07:09  ibdata1
-rw-r----- 1 mysql mysql 12582912 Aug 30 07:09  ibtmp1
-rw-r----- 1 mysql mysql      181 Aug 24 13:01  master-log-bin.000001
-rw-r----- 1 mysql mysql      181 Aug 24 13:03  master-log-bin.000002
-rw-r----- 1 mysql mysql      911 Aug 26 08:11  master-log-bin.000003
-rw-r----- 1 mysql mysql      158 Aug 30 07:09  master-log-bin.000004
-rw-r----- 1 mysql mysql       96 Aug 30 07:09  master-log-bin.index
drwxr-x--- 2 mysql mysql     4096 Aug 24 12:30  mysql
-rw-r----- 1 mysql mysql 32505856 Aug 30 07:09  mysql.ibd
lrwxrwxrwx 1 mysql mysql       27 Aug 30 07:09  mysql.sock -> /var/run/mysqld/mysqld.sock
-rw-r----- 1 mysql mysql      125 Aug 24 12:30  mysql_upgrade_history
drwxr-x--- 2 mysql mysql     4096 Aug 24 12:30  performance_schema
-rw------- 1 mysql mysql     1705 Aug 24 12:30  private_key.pem
-rw-r--r-- 1 mysql mysql      452 Aug 24 12:30  public_key.pem
-rw-r--r-- 1 mysql mysql     1112 Aug 24 12:30  server-cert.pem
-rw------- 1 mysql mysql     1705 Aug 24 12:30  server-key.pem
drwxr-x--- 2 mysql mysql     4096 Aug 24 12:30  sys
# testdb 为自己创建的数据库，在这里是一个目录
drwxr-x--- 2 mysql mysql     4096 Aug 24 12:39  testdb
-rw-r----- 1 mysql mysql 16777216 Aug 30 07:11  undo_001
-rw-r----- 1 mysql mysql 16777216 Aug 30 07:11  undo_002
bash-5.1#
```

除了 `information_schema` 这个系统数据库，其它的数据库在 `/var/bin/mysql` 数据目录下都有对应的子目录。

进入创建的 `testdb` 目录:

```shell
bash-5.1# pwd
/var/lib/mysql/testdb
bash-5.1# ls -la
total 120
drwxr-x--- 2 mysql mysql   4096 Aug 24 12:39 .
drwxr-xr-x 8 mysql root    4096 Aug 30 07:09 ..
-rw-r----- 1 mysql mysql 114688 Aug 24 12:39 emp.ibd
bash-5.1#
```

## 2.3、表在文件系统中的表示

### 2.3.1、`InnoDB` 存储引擎模式 `MySql5.5`

创建一个 `testdb` 数据库，然后在数据库下创建一张 `emp` 的表;

```shell
# 当前数据库版本 5.5.62
mysql> select version();
+-----------+
| version() |
+-----------+
| 5.5.62    |
+-----------+
1 row in set (0.00 sec)
# 创建 testdb 数据库
mysql> create database testdb;
Query OK, 1 row affected (0.00 sec)
# 使用 testdb 数据库
mysql> use testdb;
Database changed
# 创建 emp 表
mysql> create table if not exists emp(id int primary key auto_increment, lname varchar(20));
Query OK, 0 rows affected (0.01 sec)
# 查看表结构
mysql> show create table emp;
+-------+------------------------------------------------+
| Table | Create Table                                                                                                                                                 |
+-------+------------------------------------------------+
| emp   | CREATE TABLE `emp` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `lname` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+-------+------------------------------------------------+
1 row in set (0.00 sec)

# 查看数据库文件存储的目录
mysql> show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.00 sec)
```

#### 2.3.1.1、表结构文件 `.frm`

在 `mysql 5.7` 及更早版本，每张表都会在数据目录下生成一个 `.frm` 文件，用来存储表结构定义，而实际数据放在 `.ibd`（`InnoDB`）或 `.MYD/.MYI`（`MyISAM`）里;

为了保存表结构，`InnoDB` 在 数据目录 下对应数据库子目录下创建了一个专门用于 `描述表结构` 的文件，文件名结构如下

```shell
表名.frm
```

1. `.frm` 文件的格式在不同的平台上都是相同的。
2. `.frm` 是以二进制格式存储的，直接打开是乱码的;

> 查看 `/var/lib/mysql/` 数据库文件目录

```shell
root@dd23aab9ac96:/var/lib/mysql/testdb# pwd
# testdb 数据库对应的目录
/var/lib/mysql/testdb
root@dd23aab9ac96:/var/lib/mysql/testdb# ls -la
total 24
drwx------ 2 mysql mysql 4096 Aug 30 09:02 .
drwxr-xr-x 5 mysql mysql 4096 Aug 30 09:02 ..
-rw-rw---- 1 mysql mysql   65 Aug 30 09:02 db.opt
# 表结构文件
-rw-rw---- 1 mysql mysql 8588 Aug 30 09:02 emp.frm
root@dd23aab9ac96:/var/lib/mysql/testdb#
```

#### 2.3.1.2、表中的数据和索引

> 系统表空间 (`system tablespace`)

默认情况下，`InnoDB` 会在数据目录下创建一个名为 `ibdata1` 的文件，大小为 `12M`，这个文件就是对应的系统表空间在文件系统上的表示。注意这个文件是自扩展文件，当不够用的时候它会自己增加文件大小;

如果想配置系统表空间对应文件系统上多个实际文件，或者仅仅觉得原来的 `ibdata1` 这个文件名难听，那可以在 `MySQL` 启动时配置对应的文件路径以及它们的大小;

`ibdata1` 是 `InnoDB` 存储引擎的系统表空间文件，相当于 `InnoDB` 的“总仓库”。它包含了多种关键数据和元数据，是 `MySQL` 数据库运行的基础文件之一。

在 `MySQL 5.5` 及更早版本（包括 `5.5`），`ibdata1` 默认存储以下内容：

| 内容                                                    | 说明                                                                         |
| ------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 数据字典​                                               | 所有 `InnoDB` 表的元数据（表结构、列、索引定义等）                           |
| `Undo` 日志​                                            | 事务回滚所需的旧版本数据                                                     |
| `Doublewrite buffer`                                    | ​ 防止部分写损坏的写缓存                                                     |
| `Insert buffer​`                                        | 辅助索引变更的缓冲（现称 `Change Buffer`）                                   |
| 自适应哈希索引​                                         | 频繁访问的索引页的哈希索引                                                   |
| 所有表的数据和索引（当 `innodb_file_per_table=OFF` 时） | 因为没有开启独立表空间，创建的 `testdb.emp` 表的数据和索引被塞进了 `ibdata1` |

修改 `my.cnf` 配置文件

```shell
[server]
innodb_data_file_path=data1:512M;data2:512M:autoextend
```

`MySql` 启动时会按照以下顺序读取配置文件，找到第一个就停:

```shell
/etc/my.cnf
/etc/mysql/my.cnf
/usr/etc/my.cnf
~/.my.cnf
```
在 `Linux` 上最常见的位置是 `/etc/my.cnf`​ 或 `/etc/mysql/my.cnf`。`Docker` 容器里通常在 `/etc/mysql/my.cnf`，或者 `/etc/mysql/conf.d/` 下的自定义文件。



> 独立表空间 (`file-per-table tablespace`)

在 `MySQL5.6.6` 以及之后的版本中，`InnoDB` 并不会默认的把各个表的数据存储到系统表空间中，而是 **为每一个表建立一个独立表空间**，也就是说我们创建了多少个表，就有多少个独立表空间。使用独立表空间来存储表数据的话，会在该表所属数据库对应的子目录下创建一个表示该独立表空间的文件，文件名和表名相同，只不过添加了一个 `.ibd` 的扩展名而已;

```shell
表名.ibd
```

当前数据库版本为 5.5，查看 `innodb_file_per_table` 配置:

```shell
mysql> select version();
+-----------+
| version() |
+-----------+
| 5.5.62    |
+-----------+
1 row in set (0.00 sec)
# 查看 innodb_file_per_table 默认是关闭的
mysql> show variables like 'innodb_file_per_table';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | OFF   |
+-----------------------+-------+
1 row in set (0.00 sec)

mysql>

# 此时可以看到 testdb 数据库下的 emp 表并没有创建 emp.ibd 文件
root@dd23aab9ac96:/var/lib/mysql/testdb# pwd
/var/lib/mysql/testdb
root@dd23aab9ac96:/var/lib/mysql/testdb# ls -la
total 24
drwx------ 2 mysql mysql 4096 Aug 30 09:02 .
drwxr-xr-x 5 mysql mysql 4096 Aug 30 14:00 ..
-rw-rw---- 1 mysql mysql   65 Aug 30 09:02 db.opt
-rw-rw---- 1 mysql mysql 8588 Aug 30 09:02 emp.frm
root@dd23aab9ac96:/var/lib/mysql/testdb#
```


