# centos安装mysql

## 1 .下载并安装MySQL官方的 Yum Repository

> wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm

使用上面的命令就直接下载了安装用的Yum Repository，大概25KB的样子，然后就可以直接yum安装了。

> yum -y install mysql57-community-release-el7-10.noarch.rpm

  之后就开始安装MySQL服务器。

> yum -y install mysql-community-server

  这步可能会花些时间，安装完成后就会覆盖掉之前的mariadb。

至此MySQL就安装完成了，然后是对MySQL的一些设置。 



## 2. MySQL数据库设置 

  首先启动MySQL

> systemctl start  mysqld.service

  查看MySQL运行状态，运行状态如图：

> systemctl status mysqld.service

看到绿色的active(running)就是正在运行了

 此时MySQL已经开始正常运行，不过要想进入MySQL还得先找出此时root用户的密码，通过如下命令可以在日志文件中找出密码： 

> grep "password" /var/log/mysqld.log 
 
![img](/images/mysql.png) 

用这个密码登录

> mysql -uroot -p 

由于新创建好的密码要求复杂度，所以设置密码长度

>  set global validate_password_policy=0;
>
>  set global validate_password_length=1;

修改密码：

> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';

 但此时还有一个问题，就是因为安装了Yum Repository，以后每次yum操作都会自动更新，需要把这个卸载掉：

> yum -y remove mysql57-community-release-el7-10.noarch 

  此时才算真的完成了。

> flush privileges;

## 3.设置远程登录，

通过Navicat工具需要设置远程登录账号密码

> grant all privileges  on *.* to root@'%' identified by "root";

可以查看一下，不看就不看了

> select host,user,password from user;



## 4.关闭防火墙

>  //临时关闭 systemctl stop firewalld

> //禁止开机启动 systemctl disable firewalld

## 5.日志：

在my.cnf文件下的[mysql]下添加，即可启动日志，集群时id递增

> log_bin=mysql-bin
>
> server-id=1



