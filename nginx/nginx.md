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

