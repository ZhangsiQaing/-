# nginx.conf结构分析

nginx.conf文件中主要包括六块：
**main**，**events**，**http**，**server**，**location**，**upstream**
各个模块的结构顺序如下:

    main  
    events{
        ....
    }
    http{
        upstream{
            ....
        }
        server{
            location{
                ....
            }
        }
    }

* main块：主要控制nginx子进程的所属用户/用户组、派生子进程数、错误日志位置/级别、pid位置、子进程优先级、进程对应cpu、进程能够打开的文件描述符数目等
* events块：控制nginx处理连接的方式
* http块：是nginx处理http请求的主要配置模块，大多数配置都在这里面进行
* server块：是nginx中主机的配置块，可以配置多个虚拟主机
* location块：是server中对应的目录级别的控制块，可以有多个
* upstream块：是nginx做反向代理和负载均衡的配置块，可以有多个


# nginx.conf文件

```
user  nginx nginx;
worker_processes  4;

error_log  /opt/software/nginx/logs/error.log crit;
events {
	use epoll;   
	multi_accept on;
	worker_connections  65535;
}

http {
	include       mime.types;
	default_type  application/octet-stream;

	sendfile        on;
	tcp_nopush      on;
	tcp_nodelay     on;

	keepalive_timeout  20;

	gzip  on;
	gzip_disable "MSIE [1-6]\.(?!.*SV1)";

	client_header_buffer_size    1k;
	large_client_header_buffers  4 4k;

	fastcgi_connect_timeout 300;
	fastcgi_send_timeout 300;
	fastcgi_read_timeout 300;
	fastcgi_buffer_size 64k;
	fastcgi_buffers 16 64k;
	fastcgi_busy_buffers_size 128k;
	fastcgi_temp_file_write_size 128k;
	fastcgi_intercept_errors on;

	open_file_cache          max=5000  inactive=20s;
	open_file_cache_valid    30s;
	open_file_cache_min_uses 2;
	open_file_cache_errors   off;


	server {
		listen       80;
		server_name  www.58pic.com;
		root   /opt/software/nginx/html/www/58pic/;
		index  index.php index.html index.htm;

        include rewrite_www;

		# if (!-e $request_filename){
		# 	rewrite ^/(.*) /index.php last;
		# }

		location ~ \.php$ {
			fastcgi_pass   127.0.0.1:9000;
			fastcgi_index  index.php;
			fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
			include        fastcgi_params;
		}
	}
}
```
