linux二级制安装mysql
======
首先说明软件安装可以分为三类;一种是rpm安装；一种是二级制安装；一种是源代码安装；
其中源代码安装需要编译成二进制才能进行；但是与二进制安装步骤还是有所区别。


# 软件准备

*	centos6.x以上
*	mysql-5.6.27-linux-glibc2.5-i686.tar.gz

说明 mysql介质下载路径:http://ftp.ntu.edu.tw/MySQL/Downloads/MySQL-5.6/mysql-5.6.36-linux-glibc2.5-i686.tar.gz

由于应用软件现在要求的版本是5.6，故而下载5.6的介质

# 安装步骤

## 添加用户和组

	groupadd mysql;
	useradd -g mysql mysql;

## 将安装包放到/tmp路径下

略过

## 将mysql数据库文件安装到/usr/local/mysql

	cd /usr/local
	tar zxvf /tmp/mysql-5.6.27-linux-glibc2.5-i686.tar.gz
	mv  /usr/local/mysql-5.6.27-linux-glibc2.5-i686 /usr/local/mysql
	
## 修改文件所属用户组

	chown -R mysql:mysql /usr/local/mysql
	
## mysql初始化

	/usr/local/mysql/scripts/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
	cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
	
## 配置文件

	cp /usr/local/mysql/my.cnf /etc/my.cnf
	vi /etc/my.cnf
	
		basedir = /usr/local/mysql
		datadir = /usr/local/mysql/data
		port = 3306
		server_id = 1
		
## 启动mysql服务

	ln -s /usr/local/mysql/bin/mysql /usr/bin
	service mysqld start
	mysql -u root 
	
后续mysql的基本操作不在此表述

报错信息
-------

<font color="#FF0000">libaio.so.1: cannot open shared object file: No such file or directory</font>

<font color="#FF0000">libstdc++.so.6: cannot open shared object file: No such file or directory</font>

<font color="#FF0000">libncurses.so.5: cannot open shared object file: No such file or directory</font>

请安装依赖包：libstdc++.so.6,libaio.so.1,libncurses.so.5

WARNING: The host 'sqlite' could not be looked up with /usr/local/mysql/bin/resolveip.

修改hosts信息，添加ip地址映射
