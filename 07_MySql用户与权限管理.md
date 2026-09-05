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

## 1.5、设置当前用户密码

```shell
# 创建 user1 用户，密码 123
mysql> create user 'user1'@'localhost' identified by '123';
Query OK, 0 rows affected (0.02 sec)

mysql> select user, host from mysql.user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
| user1            | localhost |
+------------------+-----------+
5 rows in set (0.00 sec)

mysql>
```

> 使用 `ALTER USER` 命令来修改当前用户密码

```shell
ALTER USER USER() IDENTIFIED BY 'new_password';

mysql> select user();
+-----------------+
| user()          |
+-----------------+
| user1@localhost |
+-----------------+
1 row in set (0.00 sec)

# 修改当前用户的密码为 456，新密码下次连接的时候生效
mysql> ALTER USER USER() IDENTIFIED BY '456';
Query OK, 0 rows affected (0.01 sec)
```

> 使用 `SET` 语句来修改当前用户密码

- `MySQL 5.7` 及更早版本：`SET PASSWORD = 'new_password';` 是标准的修改当前用户密码的命令，直接生效。
- `MySQL 8.0+`：这条语句已被标记为废弃（`deprecated`），但仍然可用。官方推荐使用 `ALTER USER USER() IDENTIFIED BY 'new_password';` 替代。

```shell
# 新密码下次连接时生效，无需执行 FLUSH PRIVILEGES
mysql> set password = '789';
Query OK, 0 rows affected (0.01 sec)
```

## 1.6、修改其它用户密码

> 使用 `ALTER` 语句来修改普通用户的密码 可以使用`ALTER USER` 语句来修改普通用户的密码。基本语法形式如下：

```shell
ALTER USER user [IDENTIFIED BY '新密码'] [,user[IDENTIFIED BY '新密码']]…;
```

> 使用 `SET` 命令来修改普通用户的密码 使用`root`用户登录到`MySQL`服务器后，可以使用SET语句来修改普通用户的密码。SET语句的代码如下

```shell
SET PASSWORD FOR 'username'@'hostname'='new_password';
```


# 2、权限管理

可以使用 `show privileges;` 列出 `MySQL` 支持的所有权限及其含义,它会返回一张表格，包含: 

- `privilege` 权限名称
- `Context` 权限适用的上下文
- `Comment` 权限的说明


1. `CREATE`和`DROP`权限 ，可以创建新的数据库和表，或删除（移掉）已有的数据库和表。如果将 `MySQL` 数据库中的 `DROP` 权限授予某用户，用户就可以删除 `MySQL` 访问权限保存的数据库。
2. `SELECT`、`INSERT`、`UPDATE`和`DELETE`权限 允许在一个数据库现有的表上实施操作。 
3. `SELECT` 权限只有在它们真正从一个表中检索行时才被用到。
4. `INDEX` 权限 允许创建或删除索引，`INDEX`适用于已有的表。如果具有某个表的`CREATE`权限，就可以在`CREATE TABLE`语句中包括索引定义。
5. `ALTER` 权限可以使用 `ALTER TABLE` 来更改表的结构和重新命名表。
6. `CREATE ROUTINE` 权限 用来创建保存的程序（函数和程序），`ALTER ROUTINE` 权限用来更改和删除保存的程序，`EXECUTE` 权限 用来执行保存的程序。
7. `GRANT` 权限 允许授权给其他用户，可用于数据库、表和保存的程序。
8. `FILE` 权限 使用户可以使用 `LOAD DATA INFILE` 和 `SELECT ... INTO OUTFILE` 语句读或写服务器上的文件，任何被授予 `FILE` 权限的用户都能读或写 `MySQL` 服务器上的任何文件（说明用户可以读任何数据库目录下的文件，因为服务器可以访问这些文件）



## 2.1、授予权限

权限控制主要是出于安全因素，因此需要遵循以下几个经验原则：

1. 只授予 **能满足需要的最小权限** ，防止用户干坏事。比如用户只是需要查询，那就只给`select`权限就可以了，不要给用户赋予`update`、`insert`或者`delete` 权限。
2、创建用户的时候限制用户的登录主机，一般是限制成指定`IP`或者内网`IP段`。
3、为每个用户 **设置满足密码复杂度的密码**。
4、定期清理不需要的用户，回收权限或者删除用户

给用户授权的方式有 `2` 种，分别是通过把 **角色赋予用户给用户授权** 和 **直接给用户授权**。用户是数据库的使用者，我们可以通过给用户授予访问数据库中资源的权限，来控制使用者对数据库的访问，消除安全隐患。

```shell
# 数据库名称.* 表示授予用户这个数据库下所有表的权限
# *.* 表示授予这个用户所有数据库所有表的权限
GRANT 权限1,权限2,…权限n ON 数据库名称.表名称 TO 用户名@用户地址;

# 授予 zhangsan@localhost 用户所有数据库所有表的所有权限 （不包括 grant 权限）
grant all privileges on *.* to 'zhangsan'@'localhost'
```

### 2.1.1、给 `zhangsan` 用户授予 `testdb` 数据库下 `emp` 表的 `select, update` 权限

> step1: 在 root 用户下创建 zhangsan 账号

```shell
mysql> create user 'zhangsan'@'localhost' identified by '123456';
Query OK, 0 rows affected (0.04 sec)

mysql> select user, host from mysql.user where user='zhangsan';
+----------+-----------+
| user     | host      |
+----------+-----------+
| zhangsan | localhost |
+----------+-----------+
1 row in set (0.01 sec)

mysql>
```

在 `testdb` 数据库下，存在 `emp` 和 `user` 两张表

```shell
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
5 rows in set (0.00 sec)

mysql> use testdb;
Database changed
mysql> show tables;
+------------------+
| Tables_in_testdb |
+------------------+
| emp              |
| user             |
+------------------+
2 rows in set (0.00 sec)

mysql>
```

> step2: 登录 zhangsan 账号查看

```shell
bash-5.1# mysql -u zhangsan -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 12
Server version: 8.4.11 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# 可以看到 zhagnsan 账号只有 两个默认数据库的权限
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
+--------------------+
2 rows in set (0.00 sec)

mysql>
```

> step3: 登录 root 账号，给 zhangsan 用户授予 `testdb.emp` 表 的 `select,update` 权限

```shell
mysql> grant select, update on testdb.emp to 'zhangsan'@'localhost';
Query OK, 0 rows affected (0.02 sec)

mysql>
```

> step4: 再次查看 zhangsan 用户的数据库权限

```shell
# 可以看到 被授予了 testdb.emp 这张表的 select，update 权限
mysql> show grants;
+------------------------------------------------------------------+
| Grants for zhangsan@localhost                                    |
+------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+------------------------------------------------------------------+
2 rows in set (0.01 sec)

mysql>

# 可以看到 testdb 数据库了
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
| testdb             |
+--------------------+
3 rows in set (0.00 sec)

mysql> use testdb;
Database changed
# testdb 数据库下只能看到 emp 表
mysql> show tables;
+------------------+
| Tables_in_testdb |
+------------------+
| emp              |
+------------------+
1 row in set (0.00 sec)
# 查询数据
mysql> select * from emp;
+----+----------+
| id | lname    |
+----+----------+
|  1 | zhangsan |
|  2 | lisi     |
+----+----------+
2 rows in set (0.00 sec)

mysql>

# 删除数据没有权限
mysql> delete from emp where id = 1;
ERROR 1142 (42000): DELETE command denied to user 'zhangsan'@'localhost' for table 'emp'
mysql>
```

## 2.2、查看权限

```shell
mysql> show grants;
+------------------------------------------------------------------+
| Grants for zhangsan@localhost                                    |
+------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql>
mysql> show grants for current_user;
+------------------------------------------------------------------+
| Grants for zhangsan@localhost                                    |
+------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql> show grants for current_user();
+------------------------------------------------------------------+
| Grants for zhangsan@localhost                                    |
+------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql>
```

> 在 root 用户下，查看某个用户的权限

```shell
mysql> show grants for 'zhangsan'@'localhost';
+------------------------------------------------------------------+
| Grants for zhangsan@localhost                                    |
+------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+------------------------------------------------------------------+
2 rows in set (0.01 sec)

mysql>
```

## 2.3、收回权限

收回权限就是取消已经赋予用户的某些权限。收回用户不必要的权限可以在一定程度上保证系统的安全性。`MySQL` 中使用 `REVOKE` 语句 取消用户的某些权限。使用 `REVOKE` 收回权限之后，用户账户的记录将从 `db`、`host`、`tables_priv`和`columns_priv`表中删除，但是用户账户记录仍然在user表中保存（删除`user` 表中的账户记录使用 `DROP USER` 语句）。

```shell
# 收回权限命令格式
REVOKE 权限1,权限2,…权限n ON 数据库名称.表名称 FROM 用户名@用户地址;
```

> step1: 在 root 用户下，收回 zhangsan@localhost 用户对 testdb.emp 表的 update 权限

```shell
mysql> show grants for 'zhangsan'@'localhost';
+------------------------------------------------------------------+
| Grants for zhangsan@localhost                                    |
+------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+------------------------------------------------------------------+
2 rows in set (0.00 sec)

# 收回 zhangsan@localhost 对 testdb.emp 表的 update 权限
mysql> revoke update on testdb.emp from 'zhangsan'@'localhost';
Query OK, 0 rows affected (0.01 sec)

# 再次查询 zhangsan@localhost 用户授予的权限，可以看到 update 权限没了
mysql> show grants for 'zhangsan'@'localhost';
+----------------------------------------------------------+
| Grants for zhangsan@localhost                            |
+----------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`             |
| GRANT SELECT ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+----------------------------------------------------------+
2 rows in set (0.00 sec)

mysql>
```

> step2: 切换到之前登录的 zhangsan@localhost 用户

```shell
# 查看权限
mysql> show grants;
+----------------------------------------------------------+
| Grants for zhangsan@localhost                            |
+----------------------------------------------------------+
| GRANT USAGE ON *.* TO `zhangsan`@`localhost`             |
| GRANT SELECT ON `testdb`.`emp` TO `zhangsan`@`localhost` |
+----------------------------------------------------------+
2 rows in set (0.00 sec)

# 尝试调用 update 命令更新数据，发现没有权限
mysql> update emp set lname='lisi2' where id = 2;
ERROR 1142 (42000): UPDATE command denied to user 'zhangsan'@'localhost' for table 'emp'
mysql>
```


# 3、角色管理

角色（`Role`）管理是 `MySQL 8.0` 引入的原生特性，`5.7` 及更早版本没有内置的角色机制，只能通过逐用户 `GRANT` 来分配权限。

角色本质上是一个"命名权限集合"，先把权限赋给角色，再把角色赋给用户，从而实现批量、集中的权限管理。

```shell
-- 1. 创建角色 'app_read' 等效于 'app_read'@'%'
CREATE ROLE 'app_read', 'app_write';

-- 2. 给角色授权
GRANT SELECT ON mydb.* TO 'app_read';
GRANT INSERT, UPDATE, DELETE ON mydb.* TO 'app_write';

-- 3. 把角色分配给用户
CREATE USER 'dev1'@'%' IDENTIFIED BY 'secret';
GRANT 'app_read' TO 'dev1'@'%';

-- 4. 设置默认角色，让角色在登录时自动激活
SET DEFAULT ROLE ALL TO 'dev1'@'%';
```

## 3.1、创建 `manager` 角色，并赋值给 `lisi` 用户

> step1: `root` 下创建 `lisi'@'%' 用户

```shell
mysql> create user 'lisi' identified by '123456';
Query OK, 0 rows affected (0.04 sec)

mysql> show grants for 'lisi'@'%';
+----------------------------------+
| Grants for lisi@%                |
+----------------------------------+
| GRANT USAGE ON *.* TO `lisi`@`%` |
+----------------------------------+
1 row in set (0.00 sec)

mysql>
```

> step2: `root` 下创建 `manager'@'%' 角色

```shell
mysql> create role 'manager'@'%';
Query OK, 0 rows affected (0.02 sec)

mysql> show grants for 'manager';
+-------------------------------------+
| Grants for manager@%                |
+-------------------------------------+
| GRANT USAGE ON *.* TO `manager`@`%` |
+-------------------------------------+
1 row in set (0.00 sec)

mysql>
```

`role` 角色和用户 `lisi` 都在 `mysql.user` 表下，也就意味着 角色和用户不能重名;

```shell
mysql> select user, host from mysql.user where user in ('lisi', 'manager');
+---------+------+
| user    | host |
+---------+------+
| lisi    | %    |
| manager | %    |
+---------+------+
2 rows in set (0.01 sec)

mysql>
```

> step3: 登录 `lisi` 查看权限

```shell
bash-5.1# mysql -ulisi -p123456
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 8.4.11 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show grants;
+----------------------------------+
| Grants for lisi@%                |
+----------------------------------+
| GRANT USAGE ON *.* TO `lisi`@`%` |
+----------------------------------+
1 row in set (0.00 sec)

mysql>
# 查看数据库只能看到默认的数据库
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
+--------------------+
2 rows in set (0.02 sec)

mysql>
```

> step4: `root` 用户下给 `manager` 角色授予 `testdb.emp` 表的 `select,update` 权限

给角色授予权限的命令和给用户授予权限的命令相同

```shell
mysql> grant select, update on testdb.emp to 'manager';
Query OK, 0 rows affected (0.01 sec)

mysql> show grants for manager;
+---------------------------------------------------------+
| Grants for manager@%                                    |
+---------------------------------------------------------+
| GRANT USAGE ON *.* TO `manager`@`%`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `manager`@`%` |
+---------------------------------------------------------+
2 rows in set (0.00 sec)

mysql>
```

> step5: 给 `lisi` 授予 `manager` 角色

```shell
mysql> grant 'manager'@'%' to 'lisi'@'%';
Query OK, 0 rows affected (0.02 sec)

# 可以看到李四被授予了 manager 权限
mysql> show grants for lisi;
+-----------------------------------+
| Grants for lisi@%                 |
+-----------------------------------+
| GRANT USAGE ON *.* TO `lisi`@`%`  |
| GRANT `manager`@`%` TO `lisi`@`%` |
+-----------------------------------+
2 rows in set (0.00 sec)

mysql>
```

> step6: 切换到 `lisi` 账号下，查看权限和数据库

```shell
# 可以看到有 manager 权限
mysql> show grants;
+-----------------------------------+
| Grants for lisi@%                 |
+-----------------------------------+
| GRANT USAGE ON *.* TO `lisi`@`%`  |
| GRANT `manager`@`%` TO `lisi`@`%` |
+-----------------------------------+
2 rows in set (0.00 sec)
# 还是不能看到 testdb 数据库
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
+--------------------+
2 rows in set (0.00 sec)

mysql>
```

> step7: `root` 下激活角色

```shell
mysql> set default role 'manager'@'%' to 'lisi'@'%';
Query OK, 0 rows affected (0.01 sec)

mysql>
```

> step8: 重新登录 lisi 账号

```shell
# 切换到 lisi 账号下，还是看不到 testdb 数据库
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
+--------------------+
2 rows in set (0.00 sec)

mysql>

# 重新登录 lisi 账号

bash-5.1# mysql -ulisi -p123456
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14
Server version: 8.4.11 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
# 能够看到 testdb 数据库了
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
| testdb             |
+--------------------+
3 rows in set (0.01 sec)
# manager 角色的权限也被展开到 权限列表中了
mysql> show grants;
+------------------------------------------------------+
| Grants for lisi@%                                    |
+------------------------------------------------------+
| GRANT USAGE ON *.* TO `lisi`@`%`                     |
| GRANT SELECT, UPDATE ON `testdb`.`emp` TO `lisi`@`%` |
| GRANT `manager`@`%` TO `lisi`@`%`                    |
+------------------------------------------------------+
3 rows in set (0.01 sec)

mysql>
```






