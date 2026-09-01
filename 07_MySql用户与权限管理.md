[toc]

# 1、用户管理

## 1.1、登录 `MySql` 服务器

启动MySQL服务后，可以通过mysql命令来登录MySQL服务器

```shell
mysql –h hostname|hostIP –P port –u username –p DatabaseName –e "SQL语句"
```

- `-h` 参数 后面接主机名或者主机`IP`，`hostname`为主机，`hostIP`为主机`IP`。
- `-P` 参数 后面接`MySQL`服务的端口，通过该参数连接到指定的端口。`MySQL`服务的默认端口是`3306`，不使用该参数时自动连接到`3306`端口，`port`为连接的端口号。
- `-u` 参数 后面接用户名，`username`为用户名。
- `-p` 参数 会提示输入密码。
- `DatabaseName` 参数 指明登录到哪一个数据库中。如果没有该参数，就会直接登录到`MySQL`数据库中，然后可以使用 `USE` 命令来选择数据库。
- `-e` 参数 后面可以直接加`SQL`语句。登录`MySQL`服务器以后即可执行这个`SQ`L语句，然后退出 `MySQL`服务器。

```shell
bash-5.1# mysql -u root -p -h 127.0.0.1 -P 3306 testdb -e "select * from emp"
# -p 参数需要输入密码
Enter password:
+----+----------+
| id | lname    |
+----+----------+
|  1 | zhangsan |
|  2 | lisi     |
+----+----------+
# -e的语句执行结束后，默认会退出mysql服务器
bash-5.1#
```

## 1.2、创建用户

`CREATE USER` 语句的基本语法形式如下

```shell
CREATE USER 用户名 [IDENTIFIED BY '密码'][,用户名 [IDENTIFIED BY '密码']];
```

- 用户名: 表示新建用户的账户，由用户（`User`）和主机名（`Host`）构成；
    - 格式：`'username'@'hostname'`
    - `hostname` 指定允许登录的主机，常用值:
        - `'localhost'` – 仅本地连接
        - `'%'` – 任意主机（生产环境慎用）
        - `'192.168.1.%'` – 特定网段
    - 省略 `@'host'` 时等价于 `@'%'`
- `[]`表示可选，也就是说，可以指定用户登录时需要密码验证，也可以不指定密码验证，这样用户可以直接登录。不过，不指定密码的方式不安全，不推荐使用。如果指定密码值，这里需要使用 `IDENTIFIED BY` 指定明文密码值。
- `CREATE USER` 语句可以同时创建多个用户。

> 插入用户

```shell
mysql> create user user1 identified by '123';
Query OK, 0 rows affected (0.04 sec)

mysql> create user 'user2'@'localhost' identified by '123';
Query OK, 0 rows affected (0.02 sec)
```

> 查询用户

用户信息存放在 `mysql.user` 表;

```shell
mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| user1            | %         |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
| user2            | localhost |
+------------------+-----------+
7 rows in set (0.00 sec)
```

## 1.3、修改用户

```shell
mysql> update mysql.user set user='user_1' where user = 'user1';
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0

# 重新加载 MySQL 系统授权表到内存，使对 mysql.user、mysql.db 等系统表的直接修改（如 INSERT、UPDATE、DELETE）立即生效。
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.02 sec)

mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| user_1           | %         |    # user1 已经修改成了 user_1
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
| user2            | localhost |
+------------------+-----------+
7 rows in set (0.00 sec)
```

## 1.4、删除用户

> `drop user` // 推荐

- 标准`DDL`，自动同步删除该用户在 `mysql.db`、`tables_priv`、`columns_priv`、`procs_priv` 等所有权限表中的关联记录。
- 无需手动 `FLUSH PRIVILEGES`，一次性彻底清除用户及其权限，不留隐患。
- 适合日常管理，安全可靠。
- 使用 `DROP USER` 语句来删除用户时，必须用于 `DROP USER` 权限。

```shell
DROP USER user[,user]…;

ysql> drop user user_1;
Query OK, 0 rows affected (0.01 sec)

mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
| user2            | localhost |
+------------------+-----------+
6 rows in set (0.00 sec)

mysql>
```

> `delete from mysql.user`  // 不推荐

- 直接操作系统表的 `DML`，只删除用户表本行，其他权限表中的关联数据变成孤立记录（孤儿数据）。
- 必须手动逐一清理各权限表，并执行 `FLUSH PRIVILEGES` 才能生效。
- 容易导致权限残留、未来新建同名用户意外获得旧权限等安全问题，强烈不推荐。

```shell
DELETE FROM mysql.user WHERE Host=’hostname’ AND User=’username’

mysql> delete from mysql.user where user = 'user2' and host = 'localhost';
Query OK, 1 row affected (0.02 sec)
# 使用 delete 语句删除用户数据后，需要刷新系统授权表信息
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)

mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
5 rows in set (0.00 sec)

mysql>
```

# 2、权限管理


