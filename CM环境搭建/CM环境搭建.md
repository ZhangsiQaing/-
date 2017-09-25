### ǰ�ڻ���׼��
��¡�����:����
vi /etc/sysconfig/network-scripts/ifcfg-eth0
BOOTPROTO=none
IPV6INIT=no
IPADDR=192.168.147.138
GATEWAY=192.168.147.2
DNS1=192.168.147.2
ONBOOT="yes"

## �޸� /etc/udev/rules.d/70-persistent-net.rules   
������һ��eth1����ԭ����0ɾ�������µ�1�ĳ�0

## ����hostname
hostname cm1  ��ʱ����
vi /etc/sysconfig/network
   ���� HOSTNAME=cm1

## �رշ���ǽ������selinux�����нڵ㣩
* �رշ���ǽ
    service iptables stop
* �رտ�������
    chkconfig iptables off
* �鿴selinux״̬
    /usr/sbin/sestatus -v
* �ر�selinux��vim /etc/selinux/config
    SELINUX=disabled

## ��̨����
```
           IP           hostname
     192.168.147.137      cm1
     192.168.147.138      cm2
     192.168.147.139      cm3
```

## ʱ��ͬ��	
1����Ⱥ�����з������Ļ���ʱ���Ǳ���Ҫһ��ͬ����
2��ģ����ҵ�����Ļ�����������Ⱥ��������ʱ��ͬ��
    -���ڼ�Ⱥ����һ̨��������Ϊʱ���������ʣ�µ������ڵ�����̨����������ʱ��ͬ��
3������ntp����
    �鿴��ǰϵͳntpd������û�п���$ sudo service ntpd status
    ��������$ sudo service ntpd start
    ���ÿ�������$ sudo chkconfig ntpd on
4���޸�ϵͳ�����ļ�
    $ sudo vi /etc/ntp.conf
    ��һ���޸ģ��޸ĳ��Լ�������
    # Hosts on local network are less restricted.
    restrict 192.168.147.0 mask 255.255.255.0 nomodify notrap
    �ڶ����޸ģ�����ģ������������������Ҫ����������ע������Ĳ���
    # Please consider joining the pool (http://www.pool.ntp.org/join.html).
    #server 0.centos.pool.ntp.org
    #server 1.centos.pool.ntp.org
    #server 2.centos.pool.ntp.org
    �������޸ģ������ط���ע��ȥ��
    server  127.127.1.0     # local clock
    fudge   127.127.1.0 stratum 10
    �޸���ɱ����˳�������ntpd������Ч
    $ sudo service ntpd restart
5����дһ��crontab��ʱ����ÿʮ����ִ��һ�Σ�ע����Ҫͬ���Ļ�����Ҫдһ���ű�
##sync time
0-59/10 * * * * /usr/sbin/ntpdate bigdata-senior01.ibeifeng.com
6��ѡ�������BIOSϵͳ����ʱ��ͬ��
$ sudo vi /etc/sysconfig/ntpd
SYNC_HWCLOCK=yes
	-�����������Ƿ�����������BIOS����ʱ��ͬ��

## ж�ػ��߰�װjava
```
rpm -qa | grep java
rpm -e --nodeps xxx

���أ�����������óɰ���ľ���Դ
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
jdk����ʹ�õ���rpm��װ
rpm -ivh  ������rpm 
��Ĭ�ϰ�װ��/usr/java/jdk1.7.0_67
��/etc/profile�������û�������
#JAVA_HOME
export JAVA_HOME=/usr/java/jdk1.7.0_67
export PATH=$JAVA_HOME/bin:$PATH
```


## ��װmysql



## ��װcloudera manager��װ�����нڵ㣩
-��CM�Ĳ���
		-���ڵ�ֲ�
			cdh01	agent	server	mysql	
			cdh02	agent					namenode
			cdh03	agent					namenode(��)	resourcemanager������
		-��ʹ��tar�����а�װ�������ڰ�װhadoop
			-������cm��װĿ¼�����л�����
				mkdir -p /opt/cloudera-manager
			-����װCM����ʱ�����������л�����
				yum -y install chkconfig python bind-utils psmisc libxslt zlib sqlite cyrus-sasl-plain cyrus-sasl-gssapi fuse fuse-libs redhat-lsb
			-�����ؽ�ѹ��server�ڵ㣩
				tar -zxvf /opt/software/cloudera-manager-el6-cm5.3.6_x86_64.tar.gz -C /opt/cloudera-manager/
			-���޸������ļ�
				vim cm-5.3.6/etc/cloudera-scm-agent/config.ini 
				server_host=cm1(����ip��hostname������)
			-���ַ���agent�ڵ�
				scp -r /opt/cloudera-manager/*  cm2:/opt/cloudera-manager/
				scp -r /opt/cloudera-manager/*  cm3:/opt/cloudera-manager/
		-����ʼ��CM
			-����mysql������Ȩ
				grant all privileges on *.* to 'cmf'@'%' identified by 'cmf' with grant option;
				grant all privileges on *.* to 'cmf'@'frank-cdh01' identified by 'cmf' with grant option;
				flush privileges;
			-������jdbc
				mkdir -p /usr/share/java
				cp /opt/softwares/mysql-connector-java-5.1.26-bin.jar /usr/share/java/mysql-connector-java.jar
			-����ʼ��
			cd /opt/cloudera-manager/cm-5.3.6/share/cmf/schema
			./scm_prepare_database.sh mysql -h cm1 -ucmf -pcmf --scm-host cm1 scm scm scm
		-������parcel
			-��server������Դ�����л�����
				mkdir -p /opt/cloudera/parcel-repo
			-���ƶ�parcelԴ��������Ŀ¼�£����л�����
				mv /opt/software/CDH*  /opt/cloudera/parcel-repo/
			-������֤�ļ�������
				mv CDH-5.3.6-1.cdh5.3.6.p0.11-el6.parcel.sha1 CDH-5.3.6-1.cdh5.3.6.p0.11-el6.parcel.sha
			-����ÿ��agent�ڵ㴴����ܷ���Ŀ¼
				mkdir -p /opt/cloudera/parcels
				useradd --system --home=/opt/cloudera-manager/cm-5.3.6/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
				chown -R cloudera-scm:cloudera-scm /opt/cloudera


