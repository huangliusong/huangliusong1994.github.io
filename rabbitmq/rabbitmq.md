# rabbitmq



1.拉取镜像

> docker pull rabbitmq:3.7.7-management

2.根据下载的镜像创建和启动容器

>  docker run -d --name rabbitmq3.7.7 -p 5672:5672 -p 15672:15672 -v `pwd`/data:/var/lib/rabbitmq --hostname myRabbit -e RABBITMQ_DEFAULT_VHOST=my_vhost  -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin df80af9ca0c9

~~~
说明：

-d 后台运行容器；

--name 指定容器名；

-p 指定服务运行的端口（5672：应用访问端口；15672：控制台Web端口号）；

-v 映射目录或文件；

--hostname  主机名（RabbitMQ的一个重要注意事项是它根据所谓的 “节点名称” 存储数据，默认为主机名）；

-e 指定环境变量；（RABBITMQ_DEFAULT_VHOST：默认虚拟机名；RABBITMQ_DEFAULT_USER：默认的用户名；RABBITMQ_DEFAULT_PASS：默认用户名的密码）
~~~

3.

>使用命令：docker ps 查看正在运行容器

4.可以使用浏览器打开web管理端：http://Server-IP:15672

> admin

> admin

# 使用RabbitMQ

1.查看队列

>  rabbitmqctl  list_queues

2.查看host

> rabbitmqctl  list_vhosts

3.查看状态 

> rabbitmqctl  status

