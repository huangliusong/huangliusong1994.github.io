# redis使用

mac环境

## 安装resit
1. 查询镜像   
>  docker search redis

2. 拉取官方的镜像
> docker pull redis


3.查看一下是否成功：
> docker images


4.启动镜像：
> docker run -p 6379:6379 -d redis:latest redis-server


ps:
~~~
docker run -p 6379:6379 -v $PWD/data:/data  -d redis:3.2 redis-server --appendonly yes

命令说明：
-p 6379:6379 : 将容器的6379端口映射到主机的6379端口
-v $PWD/data:/data : 将主机中当前目录下的data挂载到容器的/data
redis-server --appendonly yes : 在容器执行redis-server启动命令，并打开redis持久化配置
~~~




> 查看容器启动情况：docker ps


连接redis的几种方式：

> docker exec -ti d0b86 redis-cli

> docker exec -ti d0b86 redis-cli -h localhost -p 6379 
> docker exec -ti d0b86 redis-cli -h 127.0.0.1 -p 6379 
> docker exec -ti d0b86 redis-cli -h 172.17.0.3 -p 6379 



~~~
docker inspect redis_s | grep IPAddress
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.3",
                    "IPAddress": "172.17.0.3",
~~~



如果连接远程：

~~~
docker exec -it redis_s redis-cli -h 192.168.1.100 -p 6379 -a your_password //如果有密码 使用 -a参数
192.168.1.100:6379> 

~~~