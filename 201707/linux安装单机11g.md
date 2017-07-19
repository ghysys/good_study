linux安装单机11g
========

oracle数据库单机需要整理出来一个文档，现在重新再虚拟机安装并且整理一个文档，需要后面在其他地方安装顺利

# 软件准备

*	centos6.5X64
*	linux.x64_11gR2_database_1of2.zip linux.x64_11gR2_database_1of2.zip

# 操作系统安装

略过

# 前期环境配置

##防火墙检查关闭

	chkconfig iptables off
	chkconfig --list iptables

##关闭selinux策略

	vim /etc/selinux/config