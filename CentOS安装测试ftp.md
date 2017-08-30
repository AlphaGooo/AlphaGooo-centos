ftp 命令适用于处理 FTP服务器 的下载数据。

## 1.检查系统是否已经安装了ftp，执行一下命令：
```
rpm -qa | grep -i ftp
```
## 2.如果没有查到任何 ftp 安装记录的话，那么就执行命令进行安装：
```
yum install ftp
```
## 3. 测试的话，找一个 ftp服务器地址（ftp.ksu.edu.tw），这服务器登陆的话，账号可以用 anonymous 无密码登录
```
ftp ftp.ksu.edu.tw
```
离开 ftp服务器 是用 bye 而不是 exit
