### 一、安装依赖库

## 安装gcc g++的依赖库
apt-get install build-essential
apt-get install libtool

## 安装 pcre依赖库
sudo apt-get update
sudo apt-get install libpcre3 libpcre3-dev

## 安装zlib依赖库
sudo apt-get install zlib1g-dev

## 安装ssl依赖库
sudo apt-get install openssl 
sudo apt-get install libssl-dev


## 二、编译nginx
### 解压nginx
```
tar -zxvf nginx-1.9.9.tar.gz
cd nginx-1.9.9
## 编译nginx
```
./configure --prefix=/opt/nginx  --user=nginx  --group=nginx  --with-http_ssl_module  --with-http_stub_status_module  --with-http_gzip_static_module   --with-pcre  
make && make install
```

```
sudo /opt/nginx/html/sbin/nginx    #启动
sudo /opt/nginx/html/sbin/nginx -t #检测配置文件是否正确 
sudo /opt/nginx/html/sbin/nginx -s stop #停止 
sudo /opt/nginx/html/sbin/nginx -s reload #重载配置文件
```


## 三、将nginx加入系统服务和开机启动项

在/etc/init.d/目录下创建nginx文件
```
#! /bin/sh
# Author: rui ding
# Modified: Geoffrey Grosenbach http://www.linuxidc.com
# Modified: Clement NEDELCU
# Reproduced with express authorization from its contributors
set -e
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DESC="nginx daemon"
NAME=nginx
DAEMON=/opt/nginx/$NAME
SCRIPTNAME=/etc/init.d/$NAME


# If the daemon file is not found, terminate the script.
test -x $DAEMON || exit 0

d_start() {
  $DAEMON || echo -n " already running"
}

d_stop() {
  $DAEMON –s quit || echo -n " not running"
}

d_reload() {
  $DAEMON –s reload || echo -n " could not reload"
}

case "$1" in
  start)
    echo -n "Starting $DESC: $NAME"
    d_start
    echo "."
  ;;
  stop)
    echo -n "Stopping $DESC: $NAME"
    d_stop
    echo "."
  ;;
  reload)
    echo -n "Reloading $DESC configuration..."
    d_reload
    echo "reloaded."
  ;;
  restart)
  echo -n "Restarting $DESC: $NAME"
  d_stop
# Sleep for two seconds before starting again, this should give the
# Nginx daemon some time to perform a graceful stop.
  sleep 2
  d_start
  echo "."
  ;;
  *)
  echo "Usage: $SCRIPTNAME {start|stop|restart|reload}" >&2
  exit 3
  ;;
esac
exit 0
```

### 添加开机启动
```
update-rc.d –f nginx defaults    添加开机自启动
update-rc.d -f nginx remove      取消开机自启动
```

方法一：sysc-rc-conf
```
sysv-rc-conf nginx on   添加开机自启动
```


