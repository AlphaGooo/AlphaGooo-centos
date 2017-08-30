# AlphaGooo-centos
个人centos技术积累，不定期更新。

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
