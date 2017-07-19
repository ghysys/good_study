linux安装单机11g
========

oracle数据库单机需要整理出来一个文档，现在重新再虚拟机安装并且整理一个文档，需要后面在其他地方安装顺利

# 软件准备

*	centos6.5X64
*	linux.x64_11gR2_database_1of2.zip linux.x64_11gR2_database_1of2.zip

# 操作系统安装

略过

# 前期环境配置

## 防火墙检查关闭

	chkconfig iptables off
	chkconfig --list iptables

## 关闭selinux策略

	vim /etc/selinux/config

## 配置yum源

略过

## 创建依赖包安装文件，授权并执行

	cd /etc/yum.repos.d/
	vim pkg_run.sh


	yum -y install   binutils-2*x86_64*
	yum -y install   glibc-2*x86_64* nss-softokn-freebl-3*x86_64*
	yum -y install   glibc-2*i686* nss-softokn-freebl-3*i686*
	yum -y install   compat-libstdc++-33*x86_64*
	yum -y install   glibc-common-2*x86_64*
	yum -y install   glibc-devel-2*x86_64*
	yum -y install   glibc-devel-2*i686*
	yum -y install   glibc-headers-2*x86_64*
	yum -y install   elfutils-libelf-0*x86_64*
	yum -y install   elfutils-libelf-devel-0*x86_64*
	yum -y install   gcc-4*x86_64*
	yum -y install   gcc-c++-4*x86_64*
	yum -y install   ksh-*x86_64*
	yum -y install   libaio-0*x86_64*
	yum -y install   libaio-devel-0*x86_64*
	yum -y install   libaio-0*i686*
	yum -y install   libaio-devel-0*i686*
	yum -y install   libgcc-4*x86_64*
	yum -y install   libgcc-4*i686*
	yum -y install   libstdc++-4*x86_64*
	yum -y install   libstdc++-4*i686*
	yum -y install   libstdc++-devel-4*x86_64*
	yum -y install   make-3.81*x86_64*
	yum -y install   numactl-devel-2*x86_64*
	yum -y install   sysstat-9*x86_64*
	yum -y install   compat-libstdc++-33*i686*
	yum -y install   compat-libcap*


	chmod 777 pkg_run.sh
	./pkg_run.sh