## 一、下载PHP7的最新版源码
php7.1.0  下载地址 http://php.net/get/php-7.0.9.tar.gz/from/a/mirror

## 二、解压

tar -zxf /tmp/php-7.1.0.tar.gz

## 三、安装相关依赖库
sudo apt-get update
sudo apt-get install libxml2-dev
sudo apt-get install build-essential
sudo apt-get install openssl 
sudo apt-get install libssl-dev 
sudo apt-get install make
sudo apt-get install curl
sudo apt-get install libcurl4-gnutls-dev
sudo apt-get install libjpeg-dev
sudo apt-get install libpng-dev
sudo apt-get install libmcrypt-dev
sudo apt-get install libreadline6 libreadline6-dev
sudo apt-get install libfreetype6-dev


## 四、源码编译
./configure --prefix=/opt/php --with-config-file-path=/opt/php/etc --with-openssl --with-gd --with-zlib --with-curl --with-libxml-dir --with-png-dir --with-jpeg-dir --with-freetype-dir --with-pear --with-gettext --enable-inline-optimization --enable-soap --enable-ftp --enable-sockets --enable-mbstring --enable-fpm --with-fpm-user=goforit --with-fpm-group=goforit --with-libdir=lib64 --with-mcrypt --with-mhash --enable-mysqlnd --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd


## 五、编译成功后的配置
```
在编译环境目录复制php.ini-production到/opt/php/etc/
cp php.ini-production  php.ini

cd /opt/php/etc  
cp php-fpm.conf.default php-fpm.conf

cd /opt/php/etc/php-fpm.d
cp www.conf.default www.conf
编辑以下内容
user = www-data
group = www-data

如果www-data用户不存在，那么先添加www-data用户
groupadd www-data  
useradd -g www-data www-data  

在nginx.conf中添加如下代码
location ~ \.php$ {
            root           html;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

启动PHP  /opt/nginx/
```
### 加入系统变量
sudo echo "PATH=$PATH:/opt/php/bin">> /etc/profile  
sudo echo "export PATH">> /etc/profile  
source /etc/profile  


## 六、添加扩展
### 添加redis扩展
```
1、下载源码
git clone -b php7 https://github.com/phpredis/phpredis.git

2、将下载下来的源码移动到/etc 文件下, 然后进入这个目录下
cd /etc/phpredis

3、执行phpize生成编译文件
$ sudo apt install php7.0-dev

中间可能会出错，使用
sudo apt-get install m4
sudo apt-get install autoconf

在/opt/php/etc/php.ini
添加如下一行
extension=redis.so

重启PHP
```

### 添加memcache扩展
```
1、下载源码
```






