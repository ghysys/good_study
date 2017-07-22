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

## 
