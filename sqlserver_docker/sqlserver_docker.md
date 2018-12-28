# SQLServer Docker

## 1.启动容器
> sudo docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=linuxG...'  -p 1433:1433 --name sql4   -d mcr.microsoft.com/mssql/server:2017-latest

## 2.在navicat中新建数据库

## 3.删除容器

>sudo docker stop sql1
>sudo docker rm sql1

## 4.从容器外连接

> sqlcmd -S 10.3.2.4,1433 -U SA -P '<YourNewStrong!Passw0rd>'

## 5.连接到 SQL Server

> sudo docker exec -it sql1 "bash"

## 6.更改 SA 密码

>sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
>  -S localhost -U SA -P '<YourStrong!Passw0rd>' \
>  -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'

[更多详情点击访问链接，访问官网文档](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-docker?view=sql-server-2017)