# ��װ������CentOS 6.4 ��nginx-1.10.3

### ����nginx
```
wget http://nginx.org/download/nginx-1.10.3.tar.gz
```

### ��װ��������
```
yum install -y gcc openssl-devel pcre-devel zlib-devel
```

### ���nginx�û�
```
useradd nginx
```

### ��ѹnginxѹ����
```
tar -zxvf nginx-1.10.3.tar.gz
```

### ����nginxĿ¼
```
cd nginx-1.10.3
```

���б���:
```
./configure --prefix=/opt/moduels/nginx --conf-path=/opt/moduels/nginx/conf/nginx.conf --error-log-path=/opt/moduels/nginx/logs/error.log --http-log-path=/opt/moduels/nginx/logs/access.log --pid-path=/opt/moduels/nginx/nginx.pid --lock-path=/opt/moduels/nginx/nginx.lock --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module  --with-http_gzip_static_module --http-client-body-temp-path=/opt/moduels/nginx/client/  --http-proxy-temp-path=/opt/moduels/nginx/proxy/  --http-fastcgi-temp-path=/opt/moduels/nginx/fcgi/  --http-uwsgi-temp-path=/opt/moduels/nginx/uwsgi/
```

�����ļ�
```
./configure 
--prefix=/opt/moduels/nginx
--sbin-path=/opt/moduels/nginx/sbin/
--conf-path=/opt/moduels/nginx/conf/nginx.conf
--error-log-path=/opt/moduels/nginx/logs/error.log 
--http-log-path=/opt/moduels/nginx/logs/access.log
--pid-path=/opt/moduels/nginx/nginx.pid
--lock-path=/opt/moduels/nginx/nginx.lock
--user=nginx
--group=nginx
--with-http_ssl_module
--with-http_stub_status_module 
--with-http_gzip_static_module
--http-client-body-temp-path=/opt/moduels/nginx/client/ 
--http-proxy-temp-path=/opt/moduels/nginx/proxy/ 
--http-fastcgi-temp-path=/opt/moduels/nginx/fcgi/ 
--http-uwsgi-temp-path=/opt/moduels/nginx/uwsgi 
```

### ͨ�����������͹ر�nginx
```
/opt/moduels/nginx/sbin/nginx      #����nginx
/opt/moduels/nginx/sbin/nginx   -s  reload   #��ֹͣ�������¶������ļ�
/opt/moduels/nginx/sbin/nginx   -s  stop     #ֹͣ����
```

## ��nginx����ϵͳ����

### ����nginx�ļ�
```  
vi nginx
```
���������ݸ��ƽ�nginx����
```
#!/bin/sh
# Source function library.
. /etc/rc.d/init.d/functions
   
# Source networking configuration.
. /etc/sysconfig/network
   
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
   
nginx="/opt/moduels/nginx/sbin/nginx"   #������Ҫ�ĳ��Լ���·��
prog=$(basename $nginx)
   
NGINX_CONF_FILE="/opt/moduels/nginx/conf/nginx.conf"    #������Ҫ�ĳ��Լ���·��
   
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
   
lockfile=/var/lock/subsys/nginx
   
make_dirs() {
   # make required directories
   user=`nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
   options=`$nginx -V 2>&1 | grep 'configure arguments:'`
   for opt in $options; do
	   if [ `echo $opt | grep '.*-temp-path'` ]; then
		   value=`echo $opt | cut -d "=" -f 2`
		   if [ ! -d "$value" ]; then
			   # echo "creating" $value
			   mkdir -p $value && chown -R $user $value
		   fi
	   fi
   done
}
   
start() {
	[ -x $nginx ] || exit 5
	[ -f $NGINX_CONF_FILE ] || exit 6
	make_dirs
	echo -n $"Starting $prog: "
	daemon $nginx -c $NGINX_CONF_FILE
	retval=$?
	echo
	[ $retval -eq 0 ] && touch $lockfile
	return $retval
}
   
stop() {
	echo -n $"Stopping $prog: "
	killproc $prog -QUIT
	retval=$?
	echo
	[ $retval -eq 0 ] && rm -f $lockfile
	return $retval
}
   
restart() {
	configtest || return $?
	stop
	sleep 1
	start
}
   
reload() {
	configtest || return $?
	echo -n $"Reloading $prog: "
	killproc $nginx -HUP
	RETVAL=$?
	echo
}
   
force_reload() {
	restart
}
   
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
   
rh_status() {
	status $prog
}
   
rh_status_q() {
	rh_status >/dev/null 2>&1
}
   
case "$1" in
	start)
		rh_status_q && exit 0
		$1
		;;
	stop)
		rh_status_q || exit 0
		$1
		;;
	restart|configtest)
		$1
		;;
	reload)
		rh_status_q || exit 7
		$1
		;;
	force-reload)
		force_reload
		;;
	status)
		rh_status
		;;
	condrestart|try-restart)
		rh_status_q || exit 0
			;;
	*)
		echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
		exit 2
esac
```

### ���ϵͳ�����Լ���������
```
cp nginx /etc/init.d/          #��nginx�ļ����ƽ�/etc/init.d/Ŀ¼��
chmod 755 /etc/init.d/nginx    #���ִ��Ȩ��
chkconfig --add nginx          #����ϵͳ����
chkconfig nginx on             #����nginx��������
```
















