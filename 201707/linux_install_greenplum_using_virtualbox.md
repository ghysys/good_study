linux 虚拟机 安装 greenplum 数据库
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


参考文档
http://www.cnblogs.com/peng-lan/p/5884290.html