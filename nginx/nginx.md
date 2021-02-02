#  nginx安装
### 下载
~~~
wget https://nginx.org/download/nginx-1.15.2.tar.gz
~~~
### 解压
~~~
tar -zxvf nginx-1.15.2.tar.gz 
cd nginx-1.15.2
~~~
### 安装
~~~
./configure 
# 编译安装(默认安装在/usr/local/nginx)
make
make install

启动Nginx：
/usr/local/nginx/sbin/nginx
~~~

## 放入html到/usr/local/nginx/html/
/usr/local/nginx/html/
然后直接访问ip地址，比如：http://192.168.0.110/
如果能看到如下Nginx主页说明安装ok。



### nginx操作
~~~
启动Nginx：
/usr/local/nginx/sbin/nginx
停止Nginx：
/usr/local/nginx/sbin/nginx -s stop
重启Nginx：
/usr/local/nginx/sbin/nginx -s reload
~~~


### iptables
~~~
nginx默认监听80端口，如果未关闭防火墙需要配置iptables规则开放80端口(以centos6为例)。
编辑配置文件：vim /etc/sysconfig/iptables
在文件中间添加iptables规则
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
重启防火墙：service iptables restart
或者关闭iptables规则：iptables -F && iptables -t nat -F
~~~

### ngnix 80->443
~~~
rewrite ^(.*)$ https://$host$1  permanent

return      301 https://$server_name$request_uri;      //这是nginx最新支持的写法
~~~
  

------------------------------

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

