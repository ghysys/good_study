linux yum install mysql
======
首先说明软件安装可以分为三类;一种是rpm安装；一种是二级制安装；一种是源代码安装；
而本次说明的yum安装其实只是rpm升级而已。

# 软件准备

无

# 安装步骤

## yum配置

略过

## 查看是否mysql的包

	#yum list installed | grep mysql

如果存在mysql包，需要移除

	#yum -y remove mysql-libs.x86_64
	
## 查看本地rpm包mysql版本

	#yum -y list mysql*
	
## 安装mysql服务器

	#yum -y install mysql-server mysql mysql-devel

## 查看mysql版本

	#rpm -qi mysql-server
	
## 启动mysqld服务

	#service mysqld start
	
## mysql登录

略过
	

# 参考资料
http://note.youdao.com/noteshare?id=91af08fdb6b648789d0a080303712121&sub=42D1117BDE0544C2AC2A10A4785B1EA9