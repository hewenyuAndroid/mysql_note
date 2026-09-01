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

# 5、数据库和文件系统的关系

## 5.1、`MySql` 自带的数据库

> mysql 5.7

```shell
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql>
```

> mysql 8.4

```shell
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.02 sec)
```

`MySql` 中有四个数据库是属于自带的系统数据库, `mysql`、`information_schema`、`performance_schema` 和 `sys`；

### 5.1.1、`mysql` 数据库

`mysql` 数据库是系统自带的核心数据库，它存储了`MySQL`的用户账户和权限信息，一些存储过程、事件的定义信息，一些运行过程中产生的日志信息，一些帮助信息以及时区信息等。

> mysql 5.7

```shell
mysql> use mysql;
Database changed
mysql> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| engine_cost               |
| event                     |
| func                      |
| general_log               |
| gtid_executed             |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| innodb_index_stats        |
| innodb_table_stats        |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| server_cost               |
| servers                   |
| slave_master_info         |
| slave_relay_log_info      |
| slave_worker_info         |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
31 rows in set (0.00 sec)

mysql>
```

> mysql 8.4

```shell
mysql> use mysql;
Database changed
mysql> show tables;
+------------------------------------------------------+
| Tables_in_mysql                                      |
+------------------------------------------------------+
| columns_priv                                         |
| component                                            |
| db                                                   |
| default_roles                                        |
| engine_cost                                          |
| func                                                 |
| general_log                                          |
| global_grants                                        |
| gtid_executed                                        |
| help_category                                        |
| help_keyword                                         |
| help_relation                                        |
| help_topic                                           |
| innodb_index_stats                                   |
| innodb_table_stats                                   |
| ndb_binlog_index                                     |
| password_history                                     |
| plugin                                               |
| procs_priv                                           |
| proxies_priv                                         |
| replication_asynchronous_connection_failover         |
| replication_asynchronous_connection_failover_managed |
| replication_group_configuration_version              |
| replication_group_member_actions                     |
| role_edges                                           |
| server_cost                                          |
| servers                                              |
| slave_master_info                                    |
| slave_relay_log_info                                 |
| slave_worker_info                                    |
| slow_log                                             |
| tables_priv                                          |
| time_zone                                            |
| time_zone_leap_second                                |
| time_zone_name                                       |
| time_zone_transition                                 |
| time_zone_transition_type                            |
| user                                                 |
+------------------------------------------------------+
38 rows in set (0.00 sec)

mysql>
```

### 5.1.2、`information_schema` 数据库

`information_schema` 数据库保存着 `MySQL` 服务器维护的所有其他数据库的信息，比如有哪些表、哪些视图、哪些触发器、哪些列、哪些索引。这些信息并不是真实的用户数据，而是一些描述性信息，有时候也称之为元数据。在系统数据库 `information_schema` 中提供了一些以 `innodb_sys` 开头的表，用于表示内部系统表。

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

> mysql 5.7

```shell
mysql> use information_schema;
Database changed
mysql> show tables like 'INNODB_SYS%';
+--------------------------------------------+
| Tables_in_information_schema (INNODB_SYS%) |
+--------------------------------------------+
| INNODB_SYS_DATAFILES                       |
| INNODB_SYS_VIRTUAL                         |
| INNODB_SYS_INDEXES                         |
| INNODB_SYS_TABLES                          |
| INNODB_SYS_FIELDS                          |
| INNODB_SYS_TABLESPACES                     |
| INNODB_SYS_FOREIGN_COLS                    |
| INNODB_SYS_COLUMNS                         |
| INNODB_SYS_FOREIGN                         |
| INNODB_SYS_TABLESTATS                      |
+--------------------------------------------+
10 rows in set (0.00 sec)

mysql>
```

> mysql 8.4

```shell
mysql> use information_schema;
Database changed
mysql> show tables like 'INNODB%';
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
31 rows in set (0.01 sec)

mysql>
```

### 5.1.3、`performance_schema` 数据库

`performance_schema` 数据库里主要保存 `MySQL` 服务器运行过程中的一些状态信息，可以用来监控 `MySQL` 服务的各类性能指标。包括统计最近执行了哪些语句，在执行过程的每个阶段都花费了多长时间，内存的使用情况等信息。

### 5.1.4、`sys` 数据库

`sys` 数据库主要是通过视图的形式把 `information_schema` 和 `performance_schema` 结合起来，帮助系统管理员和开发人员监控 `MySQL` 的技术性能。


## 5.2、数据库 (`database`) 在文件系统中的表示

除了 `information_schema` 这个系统数据库，其它的数据库在 `/var/bin/mysql` 数据目录下都有对应的子目录。

创建一个 `testdb` 数据库，里面创建一张 `emp` 表;

```sql
mysql> create database testdb;
Query OK, 1 row affected (0.00 sec)

mysql> use testdb;
Database changed
mysql> create table if not exists emp(id int primary key auto_increment, lname varchar(20));
Query OK, 0 rows affected (0.06 sec)
```

此时 `mysql` 会在 `datadir` 数据存储目录下，为 `testdb` 数据库创建一个同名的目录 `testdb`

> mysql 5.7，查看 `/var/lib/mysql/` mysql 数据存储目录

```shell
bash-4.2# pwd
/var/lib/mysql
bash-4.2# ls -la | grep testdb
drwxr-x--- 2 mysql mysql     4096 Aug 31 13:07 testdb

bash-4.2# pwd
/var/lib/mysql/testdb
bash-4.2# ls -la
total 120
drwxr-x--- 2 mysql mysql  4096 Aug 31 13:07 .
drwxr-xr-x 6 mysql root   4096 Aug 31 13:07 ..
-rw-r----- 1 mysql mysql    65 Aug 31 13:07 db.opt
-rw-r----- 1 mysql mysql  8588 Aug 31 13:07 emp.frm
-rw-r----- 1 mysql mysql 98304 Aug 31 13:07 emp.ibd
bash-4.2#
```

> mysql 8.4，查看 `/var/lib/mysql/` mysql 数据存储目录

```shell
bash-5.1# pwd
/var/lib/mysql
bash-5.1# ls -la | grep testdb
drwxr-x--- 2 mysql mysql     4096 Aug 31 13:09 testdb

bash-5.1# pwd
/var/lib/mysql/testdb
bash-5.1# ls -la
total 120
drwxr-x--- 2 mysql mysql   4096 Aug 31 13:09 .
drwxr-xr-x 8 mysql root    4096 Aug 31 13:08 ..
# 可以看到 mysql8.4 下面只有一个 emp.ibd 文件
-rw-r----- 1 mysql mysql 114688 Aug 31 13:09 emp.ibd
bash-5.1#
```

## 5.3、表 (`table`) 在文件系统中的表示

数据库在文件系统中，以目录的形式保存在 `datadir` 目录下，目录名称为数据库名称。

### 5.3.1 `mysql 5.7` 表在文件系统中的表示

```shell
bash-4.2# pwd
/var/lib/mysql/testdb
bash-4.2# ls -la
total 120
drwxr-x--- 2 mysql mysql  4096 Aug 31 13:07 .
drwxr-xr-x 6 mysql root   4096 Aug 31 13:07 ..
# 记录该数据库的字符集和排序规则，文本文件
-rw-r----- 1 mysql mysql    65 Aug 31 13:07 db.opt
# 
-rw-r----- 1 mysql mysql  8588 Aug 31 13:07 emp.frm
-rw-r----- 1 mysql mysql 98304 Aug 31 13:07 emp.ibd
bash-4.2#
```

- `emp.ibd` 二进制文件，在 `mysql5.7` 中，该文件只存储数据和索引;
- `emp.frm` 二进制文件，用于存储表结构等信息，`mysql8.0` 后该文件被移除，表结构信息保存到 `.ibd` 文件中;
- `db.opt` 是一个文本文件，用于存储该数据库的字符集和排序规则等

> `db.opt` 文本文件

```shell
bash-4.2# cat db.opt
default-character-set=latin1
default-collation=latin1_swedish_ci
bash-4.2#
```

### 5.3.2、 `mysql 8.4` 表在文件系统中的表示

```shell
bash-5.1# pwd
/var/lib/mysql/testdb
bash-5.1# ls -la
total 120
drwxr-x--- 2 mysql mysql   4096 Aug 31 13:09 .
drwxr-xr-x 8 mysql root    4096 Aug 31 13:08 ..
-rw-r----- 1 mysql mysql 114688 Aug 31 13:09 emp.ibd
bash-5.1#
```

- `emp.ibd` 二进制文件，存储数据库数据和索引，`mysql8.0` 开始 `.ibd` 文件内部新增了 `SDI (Serialized Dictionary Information)` 区，以 `JSON` 格式存储该表的元数据(表名、列定义、索引、字符集等)；

> `docker` 中的 `mysql8.4` 容器有可能没有 `/usr/bin/ibd2sdi` 文件，可以通过如下指令安装

```shell
# 第 1 步：注册 MySQL 8.4 官方 YUM 仓库
rpm -Uvh https://dev.mysql.com/get/mysql84-community-release-el9-1.noarch.rpm

# 第 2 步（你调整后的两条）：先卸 minimal，再装完整 server
microdnf remove -y mysql-community-server-minimal
microdnf install -y mysql-community-server

# 第 3 步：验证 ibd2sdi 是否就绪
ls -la /usr/bin/ibd2sdi
ibd2sdi --version
```

> 解码 `*.ibd` 文件

```shell
# 输出到控制台显示
bash-5.1# ibd2sdi /var/lib/mysql/数据库名称/表名.ibd

# 格式化输出到文件（方便查看）
ibd2sdi --dump-file=emp.json -p /var/lib/mysql/你的数据库名/表名.ibd
# 查看生成的 JSON 文件
cat emp.json | less

# 输出到控制台显示
bash-5.1# ibd2sdi /var/lib/mysql/testdb/emp.ibd
["ibd2sdi"
,
{
        "type": 1,
        "id": 367,
        "object":
                {
    "mysqld_version_id": 80411,
    "dd_version": 80300,
    "sdi_version": 80019,
    "dd_object_type": "Table",
    # dd_object 描述表结构信息
    "dd_object": {
        "name": "emp",
        "mysql_version_id": 80411,
        "created": 20260831130910,
        "last_altered": 20260831130910,
        "hidden": 1,
        "options": "avg_row_length=0;encrypt_type=N;key_block_size=0;keys_disabled=0;pack_record=1;stats_auto_recalc=0;stats_sample_pages=0;",
        "columns": [
            {
                "name": "id",
                "type": 4,
                "is_nullable": false,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": true,
                "is_virtual": false,
                # hidden=1 是用户定义的列
                "hidden": 1,
                "ordinal_position": 1,
                "char_length": 11,
                "numeric_precision": 10,
                "numeric_scale": 0,
                "numeric_scale_null": false,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": false,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "AAAAAA==",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "interval_count=0;",
                "se_private_data": "table_id=1068;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 2,
                "column_type_utf8": "int",
                "elements": [],
                "collation_id": 255,
                "is_explicit_collation": false
            },
            {
                "name": "lname",
                "type": 16,
                "is_nullable": true,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": false,
                "is_virtual": false,
                "hidden": 1,
                "ordinal_position": 2,
                "char_length": 80,
                "numeric_precision": 0,
                "numeric_scale": 0,
                "numeric_scale_null": true,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": true,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "interval_count=0;",
                "se_private_data": "table_id=1068;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 1,
                "column_type_utf8": "varchar(20)",
                "elements": [],
                "collation_id": 255,
                "is_explicit_collation": false
            },
            {
                # 隐藏列，用于记录最后一次插入或更新改行的事务Id
                "name": "DB_TRX_ID",
                "type": 10,
                "is_nullable": false,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": false,
                "is_virtual": false,
                # hidden=2 是系统隐藏的列
                "hidden": 2,
                "ordinal_position": 3,
                "char_length": 6,
                "numeric_precision": 0,
                "numeric_scale": 0,
                "numeric_scale_null": true,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": true,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "",
                "se_private_data": "table_id=1068;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 1,
                "column_type_utf8": "",
                "elements": [],
                "collation_id": 63,
                "is_explicit_collation": false
            },
            {
                # 回滚指针，指向 undo log 中该行的前一个版本
                "name": "DB_ROLL_PTR",
                "type": 9,
                "is_nullable": false,
                "is_zerofill": false,
                "is_unsigned": false,
                "is_auto_increment": false,
                "is_virtual": false,
                "hidden": 2,
                "ordinal_position": 4,
                "char_length": 7,
                "numeric_precision": 0,
                "numeric_scale": 0,
                "numeric_scale_null": true,
                "datetime_precision": 0,
                "datetime_precision_null": 1,
                "has_no_default": false,
                "default_value_null": true,
                "srs_id_null": true,
                "srs_id": 0,
                "default_value": "",
                "default_value_utf8_null": true,
                "default_value_utf8": "",
                "default_option": "",
                "update_option": "",
                "comment": "",
                "generation_expression": "",
                "generation_expression_utf8": "",
                "options": "",
                "se_private_data": "table_id=1068;",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "column_key": 1,
                "column_type_utf8": "",
                "elements": [],
                "collation_id": 63,
                "is_explicit_collation": false
            }
        ],
        "schema_ref": "testdb",
        "se_private_id": 1068,
        "engine": "InnoDB",
        "last_checked_for_upgrade_version_id": 0,
        "comment": "",
        "se_private_data": "autoinc=0;version=0;",
        "engine_attribute": "",
        "secondary_engine_attribute": "",
        "row_format": 2,
        "partition_type": 0,
        "partition_expression": "",
        "partition_expression_utf8": "",
        "default_partitioning": 0,
        "subpartition_type": 0,
        "subpartition_expression": "",
        "subpartition_expression_utf8": "",
        "default_subpartitioning": 0,
        "indexes": [
            {
                "name": "PRIMARY",
                "hidden": false,
                "is_generated": false,
                "ordinal_position": 1,
                "comment": "",
                "options": "flags=0;",
                "se_private_data": "id=158;root=4;space_id=2;table_id=1068;trx_id=1805;",
                "type": 1,
                "algorithm": 2,
                "is_algorithm_explicit": false,
                "is_visible": true,
                "engine": "InnoDB",
                "engine_attribute": "",
                "secondary_engine_attribute": "",
                "elements": [
                    {
                        "ordinal_position": 1,
                        "length": 4,
                        "order": 2,
                        "hidden": false,
                        "column_opx": 0
                    },
                    {
                        "ordinal_position": 2,
                        "length": 4294967295,
                        "order": 2,
                        "hidden": true,
                        "column_opx": 2
                    },
                    {
                        "ordinal_position": 3,
                        "length": 4294967295,
                        "order": 2,
                        "hidden": true,
                        "column_opx": 3
                    },
                    {
                        "ordinal_position": 4,
                        "length": 4294967295,
                        "order": 2,
                        "hidden": true,
                        "column_opx": 1
                    }
                ],
                "tablespace_ref": "testdb/emp"
            }
        ],
        "foreign_keys": [],
        "check_constraints": [],
        "partitions": [],
        "collation_id": 255
    }
}
}
,
{
        "type": 2,
        "id": 7,
        "object":
                {
    "mysqld_version_id": 80411,
    "dd_version": 80300,
    "sdi_version": 80019,
    "dd_object_type": "Tablespace",
    "dd_object": {
        "name": "testdb/emp",
        "comment": "",
        "options": "autoextend_size=0;encryption=N;",
        "se_private_data": "flags=16417;id=2;server_version=80411;space_version=1;state=normal;",
        "engine": "InnoDB",
        "engine_attribute": "",
        "files": [
            {
                "ordinal_position": 1,
                "filename": "./testdb/emp.ibd",
                "se_private_data": "id=2;"
            }
        ]
    }
}
}
]
bash-5.1#
```

### 5.3.3、系统表空间 (`ibdata1`) 和用户表空间 (`*.ibd`)

- 系统表空间（`System Tablespace`）：`InnoDB` 的全局共享表空间，对应物理文件 `/var/lib/mysql/ibdata1`（可配置多个）。存储所有 `InnoDB` 表共用的元数据（数据字典、事务系统、`Undo` 日志、`Doublewrite Buffer`、`Change Buffer` 等）。无论 `innodb_file_per_table` 设置如何，系统表空间始终存在。
- 用户表空间（`User Tablespace`）：每表独立表空间（`File-per-table`），即 `/var/lib/mysql/库名/表名.ibd` 文件。每个 `InnoDB` 表拥有独立的用户表空间，存储该表的数据行和索引。可通过 `innodb_file_per_table` 参数控制是否启用（ `MySQL 5.6.7+` 默认 `ON` ）。


默认情况下，`InnoDB` 会在数据目录下创建一个名为 `ibdata1` 的文件，大小为 `12M`，这个文件就是对应的系统表空间在文件系统上的表示。注意这个文件是自扩展文件，当不够用的时候它会自己增加文件大小;

- `innodb_file_per_table=ON`（`MySQL 5.6.7+` 默认）：用户表数据和索引从 `ibdata1` 剥离到各自 `.ibd`，实现 `ibdata1 瘦身；但 ibdata1 仍保留全局基础设施。
- `5.5` 及之前默认 `OFF`：所有用户表数据存入 `ibdata1`，导致其膨胀且难以回收。







