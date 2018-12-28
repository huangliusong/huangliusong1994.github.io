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



# sqlserver系统表的使用

## 1.利用sysobjects系统表

> select * from sysobjects where xtype='U'

## 2.利用Sql语句查询数据中的所有存储过程

> select * from sysobjects where xtype='P'



## 3.查询数据库中所有的用户表

>  select * from sys.tables

## 4.查询所有的库名字

> select * from sysdatabases;

可以用这个：

> SELECT name FROM  master..sysdatabases

## 5.生成查询语句

~~~
--Generate SQL to query all table names
SELECT 'use '+name,'  SELECT name as tablename,'''+name+''' as dbname FROM SysObjects Where XType=''U'' ' 
FROM  master..sysdatabases where name like '%库模糊匹配%'


--Generate SQL to query all table SP
SELECT 'use '+name,'  SELECT name as tablename,'''+name+''' as dbname FROM SysObjects Where XType=''P'' ' 
FROM  master..sysdatabases where name like '%库模糊匹配%'
~~~

