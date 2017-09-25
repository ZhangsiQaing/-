### 前期机器准备
克隆虚拟机:操作
vi /etc/sysconfig/network-scripts/ifcfg-eth0
BOOTPROTO=none
IPV6INIT=no
IPADDR=192.168.147.138
GATEWAY=192.168.147.2
DNS1=192.168.147.2
ONBOOT="yes"

## 修改 /etc/udev/rules.d/70-persistent-net.rules   
会新增一条eth1，把原来的0删除，把新的1改成0

## 配置hostname
hostname cm1  临时配置
vi /etc/sysconfig/network
   设置 HOSTNAME=cm1

## 关闭防火墙，禁用selinux（所有节点）
* 关闭防火墙
    service iptables stop
* 关闭开机启动
    chkconfig iptables off
* 查看selinux状态
    /usr/sbin/sestatus -v
* 关闭selinux：vim /etc/selinux/config
    SELINUX=disabled

## 三台机器
```
           IP           hostname
     192.168.147.137      cm1
     192.168.147.138      cm2
     192.168.147.139      cm3
```

## 时间同步	
1、集群中所有服务器的机器时间是必须要一致同步的
2、模拟企业内网的环境，操作集群服务器的时间同步
    -》在集群中找一台服务器作为时间服务器，剩下的其他节点与这台服务器进行时间同步
3、服务：ntp服务
    查看当前系统ntpd服务有没有开启$ sudo service ntpd status
    开启服务$ sudo service ntpd start
    设置开机启动$ sudo chkconfig ntpd on
4、修改系统配置文件
    $ sudo vi /etc/ntp.conf
    第一处修改：修改成自己的网段
    # Hosts on local network are less restricted.
    restrict 192.168.147.0 mask 255.255.255.0 nomodify notrap
    第二处修改：由于模拟是内网环境，不需要连接外网，注释下面的参数
    # Please consider joining the pool (http://www.pool.ntp.org/join.html).
    #server 0.centos.pool.ntp.org
    #server 1.centos.pool.ntp.org
    #server 2.centos.pool.ntp.org
    第三处修改：将本地服务注释去掉
    server  127.127.1.0     # local clock
    fudge   127.127.1.0 stratum 10
    修改完成保存退出，重启ntpd服务，生效
    $ sudo service ntpd restart
5、编写一个crontab定时任务，每十分钟执行一次，注意需要同步的机器都要写一个脚本
##sync time
0-59/10 * * * * /usr/sbin/ntpdate bigdata-senior01.ibeifeng.com
6、选配置项：与BIOS系统进行时间同步
$ sudo vi /etc/sysconfig/ntpd
SYNC_HWCLOCK=yes
	-》参数代表是否设置启用与BIOS进行时间同步

## 卸载或者安装java
```
rpm -qa | grep java
rpm -e --nodeps xxx

下载，命令，可以配置成阿里的镜像源
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
jdk我们使用的是rpm安装
rpm -ivh  ………rpm 
他默认安装在/usr/java/jdk1.7.0_67
在/etc/profile里面配置环境变量
#JAVA_HOME
export JAVA_HOME=/usr/java/jdk1.7.0_67
export PATH=$JAVA_HOME/bin:$PATH
```


## 安装mysql



## 安装cloudera manager安装（所有节点）
-》CM的部署
		-》节点分布
			cdh01	agent	server	mysql	
			cdh02	agent					namenode
			cdh03	agent					namenode(备)	resourcemanager（备）
		-》使用tar包进行安装，类似于安装hadoop
			-》创建cm安装目录（所有机器）
				mkdir -p /opt/cloudera-manager
			-》安装CM运行时的依赖（所有机器）
				yum -y install chkconfig python bind-utils psmisc libxslt zlib sqlite cyrus-sasl-plain cyrus-sasl-gssapi fuse fuse-libs redhat-lsb
			-》下载解压（server节点）
				tar -zxvf /opt/software/cloudera-manager-el6-cm5.3.6_x86_64.tar.gz -C /opt/cloudera-manager/
			-》修改配置文件
				vim cm-5.3.6/etc/cloudera-scm-agent/config.ini 
				server_host=cm1(根据ip或hostname来配置)
			-》分发到agent节点
				scp -r /opt/cloudera-manager/*  cm2:/opt/cloudera-manager/
				scp -r /opt/cloudera-manager/*  cm3:/opt/cloudera-manager/
		-》初始化CM
			-》对mysql进行授权
				grant all privileges on *.* to 'cmf'@'%' identified by 'cmf' with grant option;
				grant all privileges on *.* to 'cmf'@'frank-cdh01' identified by 'cmf' with grant option;
				flush privileges;
			-》放置jdbc
				mkdir -p /usr/share/java
				cp /opt/softwares/mysql-connector-java-5.1.26-bin.jar /usr/share/java/mysql-connector-java.jar
			-》初始化
			cd /opt/cloudera-manager/cm-5.3.6/share/cmf/schema
			./scm_prepare_database.sh mysql -h cm1 -ucmf -pcmf --scm-host cm1 scm scm scm
		-》配置parcel
			-》server端配置源（所有机器）
				mkdir -p /opt/cloudera/parcel-repo
			-》移动parcel源到创建的目录下（所有机器）
				mv /opt/software/CDH*  /opt/cloudera/parcel-repo/
			-》将验证文件重命名
				mv CDH-5.3.6-1.cdh5.3.6.p0.11-el6.parcel.sha1 CDH-5.3.6-1.cdh5.3.6.p0.11-el6.parcel.sha
			-》在每个agent节点创建框架服务目录
				mkdir -p /opt/cloudera/parcels
				useradd --system --home=/opt/cloudera-manager/cm-5.3.6/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
				chown -R cloudera-scm:cloudera-scm /opt/cloudera


