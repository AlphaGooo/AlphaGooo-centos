# AlphaGooo-centos
- [U盘安装CentOs6.5](files/U盘安装CentOs6.5.md) - 怎么用U盘一步步往你的电脑安装 CentOS6.5
- [CentOS安装Mysql5.7](files/ConetOS安装Mysql5.7.md)  - 怎样在你的CentOS安装Mysql5.7
- [CentOS测试安装ftp](files/CentOS测试安装ftp.md)  - CentOS测试安装ftp



## CentOS 不能上网的一般解决方案：

系统安装重启后，不能上网的解决方案：

修改配置文件：vi /etc/sysconfig/network-scripts/ifcfg-eth0

把 ONBOOT配置成 true 即可。

保存修改后，重启网络服务：/etc/init.i/network restart

## CentOS 检查系统基本信息

1. 查看系统ip信息：ifconfig

2. 查看系统服务的启动状况：LANG=C chkconfig --list | grep '3:on'

3. 查询已启动的网络监听服务：netstat -tulnp

4. 查询某个软件是否被安装（如samba）： rpm -qa | grep -i samba

5. 利用yum搜索一下有没有该软件：yum search samba

6. 找出软件的配置文件：rpm -qc samba samba-common

## 文字接口下载器

centos 可以通wget 进行网页数据的获取（下载），相当于 ftp 下载数据。
```
wget [option] [网址]
```
