[toc]

MySql


# mysql 基础



环境变量配置

```text
在 path 环境变量配置 mysql bin 目录

C:\Program Files\MySQL\MySQL Server 8.0\bin
```


使用 `mysql -V` 或 `mysql --version` 查看版本号

![mysql --version](./imgs/mysql-version.png)


## 登录 mysql

| 命令 | 说明 | 示例 |
| -- | -- | -- |
| `mysql -u 用户 -p` | 本地登录 | `mysql -u root -p` |
| `mysql -u 用户 -P 端口 -h 主机地址 -p` | 远程登录 | `mysql -u root -P 3306 -h 127.0.0.1 -p` |
| `exit` 或 `\q` 或 `quit` | 退出mysql ||


1. 登录本机默认端口号(3306) mysql 服务:  `mysql -u 用户名 -p 密码 `   // 注意，`-p` 后可以直接回车，使用密文登录


![mysql-login-default](./imgs/mysql-login-default.png)


2. 登录本机指定端口号 mysql 服务:  `mysql -u 用户名 -P 端口号 -p`
   

![mysql-login-set-port](./imgs/mysql-login-set-port.png)
   

1. 登录指定ip的 mysql 服务: `mysql -u 用户名 -P 端口号 -h 主机ip -p`    	// 例如: 本机 ip 可以使用  `localhost` 或 `127.0.0.1` 


![mysql-login-set-ip-port](./imgs/mysql-login-set-ip-port.png)


## 常见操作命令


### 数据库操作命令

| 命令 | 说明 | 示例 |
| -- | -- | -- |
| `show databases;` | 查看所有数据库 ||
| `create database 数据库名;` | 创建数据库 | `create database testdb;` |
| `use 数据库名;` | 使用/切换数据库 | `use testdb;` |
| `drop database 数据库名` | 删除数据库 | `drop database testdb;` |


1. `show databases;` 查看所有数据库
   
![show databases](./imgs/mysql-cmd-show-databases.png)

2. `create database 数据库名;` 创建数据库

![create database](./imgs/mysql-cmd-create-database.png)

3. `use 数据库名;` 使用数据库
   
![use 数据库名](./imgs/mysql-cmd-use-database.png)

4. `drop database 数据库名;` 删除数据库
   
![drop database 数据库名](./imgs/mysql-cmd-drop-database.png)


















