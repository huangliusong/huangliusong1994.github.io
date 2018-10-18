# nginx配置

1.查看编译参数。 

>  nginx -V

2.ngx_http_stub_status监控连接信息配置

~~~
location =/nginx_status{
    stub_status on;
    access_log off;
    allow 127.0.0.1;
    deny all;
}
~~~

3.docker中的nginx没有vi命令？

~~~
apt-get update
apt-get install vim
apt-get install vi
~~~

4.关闭nginx

> nginx -s stop

5.启动nginx

> nginx -s start

6.重启nginx

> nginx -s reload

## nginxtop安装&使用

1.install python-pip

> yum install peel-release

> yum install python-pip

2.install ngxtop

> pip install ngxtop

3.nginxtop:指定配置文件

> ngxtop -c /etc/nginx/nginx.conf

4.查询状态是200:

> ngxtop -c /etc/nginx/nginx.conf -i 'status==200'

5.查询Ip最多的访问

> ngxtop -c /etc/nginx/nginx.conf -g remote_addr

## nginx-rrd监控

1.安装php

> yum install php php-gd php-soap php-mbstring php-xmlrpc php-dom php-fpm -y

2.nginx整合php-fpm

> 修改/etc/php-fpm.d/www.conf文件中的user和group 与nginx.conf中的user一致

~~~
user=nginx
group=nginx
~~~

4.启动php-fpm服务

systemctl start php-fpm

~~~
location ~ \.php$ {
	root /usr/share/nginx/html;
	fastcgi_pass 127.0.0.1:9000;
	fastcgi_index index.php;
	fastcgi_param SCRIPT_FILENAME $document_root/$fastcgi_script_name;
	include fastcgi_params;
}
~~~

