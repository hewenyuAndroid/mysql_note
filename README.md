MySql



# mysql 基础



环境变量配置

```text
在 path 环境变量配置 mysql bin 目录

C:\Program Files\MySQL\MySQL Server 8.0\bin
```



`mysql --version` 查看版本号

![mysql --version](./imgs/mysql-version.png)



## 登录 mysql



1. 登录本机默认端口号(3306) mysql 服务:  `mysql -u 用户名 -p 密码 `   // 注意，`-p` 后可以直接回车，使用密文登录



![mysql-login-default](./imgs/mysql-login-default.png)



2. 登录本机指定端口号 mysql 服务:  `mysql -u 用户名 -P 端口号 -p`

   

   ![mysql-login-set-port](./imgs/mysql-login-set-port.png)

   

3. 登录指定ip的 mysql 服务: `mysql -u 用户名 -P 端口号 -h 主机ip -p`    	// 例如: 本机 ip 可以使用  `localhost` 或 `127.0.0.1` 



![mysql-login-set-ip-port](./imgs/mysql-login-set-ip-port.png)













