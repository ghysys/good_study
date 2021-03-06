﻿linux 虚拟机 安装 greenplum 数据库
======

greenplum现在是开源数据库，现在很多项目都使用该数据库作为数据仓库，因此在虚拟机上面安装以便后续使用，此安装过程是我在201609左右测试成功，后面去实际环境中安装数据库，多有不同之处，这个需要好好注意

# 软件准备

* centos6.5
* greenplum-db-4.2.6.0-build-2-RHEL5-x86_64.zip

# 环境配置

## 基础信息配置

1.	节点名	主机ip	内存	空间大小
2.	master	192.168.1.80	2048	50G
3.	slave1	192.168.1.81	1536	35G
4.	slave2	192.168.1.82	1536    35G
	
## 系统配置

每个机器都要执行

### ip设置

略过

### 主机名网关设置

略过

### 防火墙关闭

略过

### 修改/etc/hosts

	#vi /etc/hosts
	192.168.30.180 master 
	192.168.30.181 slave1 
	192.168.30.182 slave2

### 系统参数配置

	#vim /etc/sysctl.conf 
	
	xfs_mount_options = rw,noatime,inode64,allocsize=16m 
	kernel.shmmax = 500000000 
	kernel.shmall = 4000000000 
	kernel.shmmni = 4096 
	kernel.sem = 250 512000 100 2048 
	kernel.sysrq = 1 
	kernel.core_uses_pid = 1 
	kernel.msgmni = 2048 
	net.ipv4.tcp_tw_recycle = 1 
	net.ipv4.tcp_max_syn_backlog = 4096 
	net.ipv4.conf.all.arp_filter = 1 
	net.ipv4.ip_local_port_range = 1025 65535 
	net.core.netdev_max_backlog = 10000 
	vm.overcommit_memory = 2 
	

	#vim /etc/security/limits.conf 
	
	* soft nofile 65536 
	* hard nofile 65536 
	* soft nproc 131072 
	* hard nproc 131072 

	
## 主机节点设置


### 解压安装包

	unzip greenplum-db-4.3.9.1-build-1-rhel5-x86_64.zip
	./greenplum-db-4.3.9.1-build-1-rhel5-x86_64.bin
	
可以选择默认，默认安装路径在/usr/local/greenplum-db-4.3.9.1

### 在安装路径下创建文件

	#vim all_hosts
	
	master 
	slave1 
	slave2
	
	#ssh slave1
	#ssh slave2

### 执行环境变量

	#source greenplum_path.sh

### 其他节点用户目录创建

	#gpseginstall -f all_hosts -u gpadmin -p gpadmin
	
## 所有主机上初始化配置Greenplum

	#su - gpadmin
	$cd /usr/local/greenplum-db 
	$source greenplum_path.sh
	$gpssh -f all_hosts -e ls -l $GPHOME
	
	$vim .bashrc 
	source /usr/local/greenplum-db/greenplum_path.sh 
	
	$scp .bashrc slave1:`pwd` 
	$scp .bashrc slave2:`pwd` 
	
## 创建数据目录并授权

	#mkdir -p /data/master 
	#chown gpadmin /data/master
	
	#vim seg_hosts 
	slave1 
	slave2
	
	
	#gpssh -f seg_hosts -e 'mkdir -p /data/primary' 
	#gpssh -f seg_hosts -e 'mkdir -p /data/mirror' 
	
	
	#gpssh -f seg_hosts -e 'chown gpadmin /data/primary' 
	#gpssh -f seg_hosts -e 'chown gpadmin /data/mirror' 
	

## 同步时间

master配置
	#vim /etc/ntp.conf 
	server 127.127.1.0 
	includefile /etc/ntp/crypto/pw 
	
slave配置
	#vim /etc/ntp.conf 
	server master 
	includefile /etc/ntp/crypto/pw 

master,slave执行ntp服务
	#gpssh -f all_hosts -v -e 'ntpd' 

## 验证系统配置

	#gpcheck -f all_hosts -m master

## 初始化Greenplum	

### 创建Greenplum数据库配置文件
	
	#su - gpadmin 
	$cp $GPHOME/docs/cli_help/gpconfigs/gpinitsystem_config /home/gpadmin/ 
	$chmod 755 /home/gpadmin/gpinitsystem_config
	
	$vim gpinitsystem_config 
	ARRAY_NAME="EMC Greenplum DW" 
	SEG_PREFIX=gpseg 
	PORT_BASE=40000
	declare -a DATA_DIRECTORY=(/data/primary) 
	MASTER_HOSTNAME=master 
	MASTER_PORT=5432 
	CHECK_POINT_SEGMENTS=8 
	ENCODING=UNICODE 
	MIRROR_PORT_BASE=50000 
	REPLICATION_PORT_BASE=41000 
	declare -a MIRROR_DATA_DIRECTORY=(/data/mirror) 
	
### 初始化数据库

	$cp /usr/local/greenplum-db/seg_hosts .
	$gpinitsystem -c gpinitsystem_config -h seg_hosts
	
## 查看数据库是否安装成功

	$ps -ef|grep post 




参考文档

http://www.cnblogs.com/peng-lan/p/5884290.html

http://note.youdao.com/noteshare?id=4c87eabf97c1d0f634e48bf2e392387f&sub=C725D5302B4A4A85B9D29A9E284C9484

附件

centos7 生产库 安装 (https://github.com/ghysys/good_study/blob/master/201707/resource/GP%E5%9C%A8CENTOS7%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2.rar)