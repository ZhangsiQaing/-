# 安装环境：CentOS 6.4 、nginx-1.10.3

### 下载nginx
```
wget http://nginx.org/download/nginx-1.10.3.tar.gz
```

### 安装依赖服务
```
yum install -y gcc openssl-devel pcre-devel zlib-devel
```

### 添加nginx用户
```
useradd nginx
```

### 解压nginx压缩包
```
tar -zxvf nginx-1.10.3.tar.gz
```

### 进入nginx目录
```
cd nginx-1.10.3
```

进行编译:
```
./configure --prefix=/opt/moduels/nginx --conf-path=/opt/moduels/nginx/conf/nginx.conf --error-log-path=/opt/moduels/nginx/logs/error.log --http-log-path=/opt/moduels/nginx/logs/access.log --pid-path=/opt/moduels/nginx/nginx.pid --lock-path=/opt/moduels/nginx/nginx.lock --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module  --with-http_gzip_static_module --http-client-body-temp-path=/opt/moduels/nginx/client/  --http-proxy-temp-path=/opt/moduels/nginx/proxy/  --http-fastcgi-temp-path=/opt/moduels/nginx/fcgi/  --http-uwsgi-temp-path=/opt/moduels/nginx/uwsgi/
```

编译文件
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

### 通过命令启动和关闭nginx
```
/opt/moduels/nginx/sbin/nginx      #启动nginx
/opt/moduels/nginx/sbin/nginx   -s  reload   #不停止服务重新读配置文件
/opt/moduels/nginx/sbin/nginx   -s  stop     #停止服务
```

## 将nginx加入系统服务

### 创建nginx文件
```  
vi nginx
```
将以下内容复制进nginx保存
```
#!/bin/sh
# Source function library.
. /etc/rc.d/init.d/functions
   
# Source networking configuration.
. /etc/sysconfig/network
   
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
   
nginx="/opt/moduels/nginx/sbin/nginx"   #这里需要改成自己的路径
prog=$(basename $nginx)
   
NGINX_CONF_FILE="/opt/moduels/nginx/conf/nginx.conf"    #这里需要改成自己的路径
   
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

### 添加系统服务以及开机启动
```
cp nginx /etc/init.d/          #将nginx文件复制进/etc/init.d/目录下
chmod 755 /etc/init.d/nginx    #添加执行权限
chkconfig --add nginx          #加入系统服务
chkconfig nginx on             #设置nginx开机启动
```
















