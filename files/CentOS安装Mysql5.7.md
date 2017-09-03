## 1.清理旧版本
CentOS 6.5 默认yum只能安装mysql 5.1 ，（这个版本比较旧）
安装前要检查机器原来是否安装过mysql，如有安装需要先进行数据备份、清理：
```
[root@snails ~]# yum list installed | grep mysql
[root@snails ~]# ps -ef|grep mysql
[root@snails ~]# service mysqld stop 
[root@snails ~]# rpm -e mysql-libs --nodeps
[root@snails ~]# yum -y remove mysql mysql-*
```


## 2.设置安装源
```
[root@snails ~]# wget http://repo.mysql.com/mysql57-community-release-el6-8.noarch.rpm
[root@snails ~]# rpm -ivh mysql57-community-release-el6-8.noarch.rpm
[root@snails ~]# ls -1 /etc/yum.repos.d/mysql-community*
[root@snails ~]# yum repolist all | grep mysql
[root@snails ~]# vi /etc/yum.repos.d/mysql-community.repo
### 将[mysql56-community]的enabled设置为１,[mysql57-community]的enabled设置为0 ### 
[root@snails ~]# yum repolist enabled | grep mysql
mysql-connectors-community MySQL Connectors Community                         21
mysql-tools-community      MySQL Tools Community                              37
mysql56-community          MySQL 5.6 Community Server                        265
```


## 3.执行安装 mysql
```
[root@snails ~]# yum -y install mysql-server mysql
```


## 3.启动服务
```
[root@snails ~]# service mysqld start
Starting mysqld:[ OK ]
```
查看 MySQL 服务状态
```
[root@snails ~]# service mysqld status
mysqld (pid  xxxx) is running...
```
## 4.开机启动
```
[root@snails ~]# chkconfig mysqld on
```

## 5.修改 root 默认密码(Lin5872528xxlin.)

MySQL 5.7 启动后，在 /var/log/mysqld.log 文件中给 root 生成了一个默认密码。通过下面的方式找到 root 默认密码，然后登录 mysql 进行修改：
```
[root@snails ~]# grep 'temporary password' /var/log/mysqld.log
host: XXXXXXXX
```
登录 MySQL 并修改密码
```
[root@snails ~]# mysql -uroot -p
Enter password: 

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
注意：MySQL 5.7 默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于 8 位。

通过 MySQL 环境变量可以查看密码策略的相关信息：
```
mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password_check_user_name    | OFF    |
| validate_password_dictionary_file    |        |
| validate_password_length             | 8      |
| validate_password_mixed_case_count   | 1      |
| validate_password_number_count       | 1      |
| validate_password_policy             | MEDIUM |
| validate_password_special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)
```

指定密码校验策略
```
[root@snails ~]# vi /etc/my.cnf
# 添加如下键值对, 0=LOW, 1=MEDIUM, 2=STRONG
validate_password_policy=0
```
禁用密码策略
```
[root@snails ~]# vi /etc/my.cnf
# 禁用密码校验策略
validate_password = off
```
重启 MySQL 服务，使配置生效:
```
[root@snails ~]# service mysqld restart
```
## 6.添加远程登录用户
MySQL 默认只允许 root 帐户在本地登录，如果要在其它机器上连接 MySQL，必须修改 root 允许远程连接，或者添加一个允许远程连接的帐户，为了安全起见，本例添加一个新的帐户：
```
mysql> GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' IDENTIFIED BY 'secret' WITH GRANT OPTION;
```
## 7.配置默认编码为 utf8
MySQL 默认为 latin1, 一般修改为 UTF-8
```
[root@snails ~]# vi /etc/my.cnf
[mysqld]
# 在myslqd下添加如下键值对
character_set_server=utf8
init_connect='SET NAMES utf8'
```
重启 MySQL 服务，使配置生效
```
[root@snails ~]# service mysqld restart
```
查看字符集
```
mysql> SHOW VARIABLES LIKE 'character%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```
