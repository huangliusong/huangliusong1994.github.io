# docker使用

##安装并启动镜像
> docker run -d -p 9200:9200 elasticsearch
## 关闭
> docker kill 8canfdsf
## 查看镜像
> docker images
## 查看运行情况
> docker ps -a
## 移除镜像
> docker rm 65e94723f0ed
## 再移除
> docker rmi a061cf8c12b8

~~~
可以根据提示来的，加-f强制删除镜像。 
顺便提一下，docker命令： 
1.docker rm <容器ID或容器名> 
2.docker stop<容器ID或容器名>


引申：
1.如何删除所有容器；
2.如何删除所有容器镜像，
3.在创建镜像时会产生很多的中间镜像，这部分镜像是一起删除的吗？none镜像？
~~~

## docker使用

> uname -r      大于3.3

> su 

> apt-get update

> sudo apt-get update

> curl -s https://get.docker.com|sh

> docker version

- 拉取镜像
> docker pull [options] name [:tag]
> docker pull [OPTIONS] name [:TAG]
> sudo docker images

- 查看本机有什么镜像
> docker images [OPTIONS] [REPOSITORY[:TAG]]

- 拉去一个镜像名字为huangliusong
> sudo docker pull hello-world

>docker pull hub.c.163.com/library/centos:latest


-运行镜像
> docker run[OPTIONS] IMAGE [:TAG] [COMMAND] [ARG:]

> docker ps 

- 后台运行
> docker run -d 

进入容器内部
> docker exec -it f4 bash

退出容器
> exit

# docker网络
~~~
网络类型 Bridge Host None
端口映射 
~~~

- 主机端口为8080容器端口为80
>docker run -d -p 8080:80 容器名字

- 查看端口
> netstat -na|grep 8080

-大P
> docker run -d -P 容器名字

# 制作自己的镜像
~~~
Dockerfile
docker build 
Jpress: http://jpress.io/
~~~
1. vi Dockerfile
from  hub.c.163.com/library/tomcat:latest
2. docker build .
3. 加上名字 docker build -t huangliusong:latest .
4. 运行自己的docker： docker run -d -p 8888:8080 huangliusong
5. docker ps
6. 查询一下端口netstat -na|grep 8888
7.启动mysql容器 docker run  -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=111111 -e MYSQL_DATABASE=zrlog  hub.c.163.com/library/mysql:latest
8.停止docker sudo docker stop 42
9.重启 docker restart 12fjkfdjkf

centos搭建Docker
http://note.youdao.com/noteshare?id=344b45198af274752fb54356fe66d3d3&sub=A90E474AC8954737BD3F4CAD2253AB12




7：ABCD 8:D 9:ABCDE 10:AC 11:D 12:A 13: CE14:BCDE 15:A 16:ABCDEFG

 mysql -uapp_net -p123456 -P8066 -h192.168.1.135

https://blog.csdn.net/tang_jin2015/article/details/78898780