# MAC 端口被占用
1.查看端口占用的进程pid  

> lsof -i tcp:8080

2.杀死进程

> kill pid
>
> 或
>
> kill -9 pid