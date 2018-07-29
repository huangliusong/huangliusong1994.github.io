## 安装
> wget http://dl.mycat.io/1.6.5/Mycat-server-1.6.5-release-20180122220033-linux.tar.gz


> tar zxf Mycat-server-1.6.5-release-20180122220033-linux.tar.gz

> http://7dx.pc6.com/wwb5/jdk7u79linuxx64.tar.gz

java环境变量配置

~~~
export PATH=$PATH:/home/mysql/bin:/usr/local/mycat/bin:/usr/local/java/jdk1.7.0_79/bin
export JAVA_HOME=/usr/local/java/jdk1.7.0_79/
export CLASSPATH=.$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export MyCAT_HOME=/usr/local/mycat
~~~



## 数据库复制步骤
~~~
使用master-data=2记录事务日志点
使用change master to 配置复制链路
使用change replicatoin filter 配置数据库名转换
~~~

## 备份数据库：
~~~
mysqldump --master-data=2 --single-transaction --routines --triggers --events -uroot -p networkmanager>n.sql

scp bak.sql root@192.168.1.3:/root
~~~

## 节点1：
### 1.建立数据库
> mysql -uroot -p <bak_imooc.sql

> mysql -uroot -p -e"create database networkmanager" 

### 2.将脚本复制到创建好的数据库中
> mysql -uroot -p networkmanager<n.sql

### 3.进入数据库中查看一下数据库是否复制好了
> mysql -uroot -p

> show databases

> ;

### 4.由于数据库重命名非常的麻烦，所以在导入的时候就应该命名好数据库，所以先删除数据库

> drop database imooc_db;

### 5.重新创建数据库
> mysql -uroot -p -e"create database order_db"

> > mysql -uroot -p order_db<bak_imooc.sql

>>mysql -uroot -p 

>>show databases

>>;

>>use order_db

>>show tables;
>

### 6.在节点一中建立一个用于主从同步的数据库账号
> mysql -uroot -p 

创建一个账号可以在1网段都可以使用的账号密码
> create user 'net_repl'@'192.168.1.%'  identified by '123456';

账号授权
> grant replication	slave on *.*  to 'net_repl'@'192.168.1.%';
>
> GRANT REPLICATION SLAVE ON *.* TO 'net_repl'@'192.169.1.%' IDENTIFIED BY '123456'; 
>
> grant replication slave on *.* to net_repl@'192.168.1.%';

至此完成了主数据库的初始化。

## 节点2： （建立数据库链路）
> \h change master to

> change master to master_host='192.168.1.2',master_user='im_repl',master_password='123456',MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=107;

> show slave status \G

注意：在正常情况下主数据库和从数据库的数据库名应该是一样的，由于需要垂直切分，所以在从数据库名中做了修改，如果直接启动slave，肯定会报错，所以需要建立过滤链路

> \h change replication filter 
~~~
REPLICATE_REWRITE_DB=(db_pair_list)
~~~

在这个步骤是在从数据库上做的
> change replication filter replication_rewrite_db=((imooc_db,order_db))

启动数据链路
> start slave;

查看数据链路状态
> show slave status \G;

测试是否成功，修改主数据库的一条数据，查看从数据库的数据是否修改
1.进入主数据库
> use imooc_db

> show tables;

> select * from region_info limit 10;

> update region_info set region_name='China中国'  where region_id=1;

> select * from region_info where region_id=1;

主数据库修改完成，去查看从数据库
> select * from region_info where region_id=1;

修改主数据库后从数据库也修改了。


## 节点3
在从数据库中建立数据链路：
change master to master_host='192.168.1.2',master_user='im_repl',master_password='123456',MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=107;

在从数据库中建立过滤链路
change replication filter replicate_rewrite_db=((imooc_db,product_db));

启动数据链路
start slave;

查看数据链路的状态
show slave status \G

## 节点4

创建节点4的数据链路
建立用户数据库
mysql -uroot -proot -p -e"create database customer_db"

通过脚本恢复数据库
mysql -uroot -p customer_db<bak_imooc.sql


在从数据库中建立数据链路：
change master to master_host='192.168.1.2',master_user='im_repl',master_password='123456',MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=107;

不同的是过滤链路的配置
change replication filter replicate_rewrite_db=((imooc_db,customer_db));

启动数据链路
> start slave;

查看数据库链路的状态
> show slave status \G



# 垂直切分数据库

## mycat配置 vi schema.xml
> cd /usr/local/mycat/
> cd conf
> vi schema.xml

1.配置schema.xml
~~~

<schema name="imooc_db" checkSQLSchema="false" sqlMaxLimit="100" >
	<table name="order_master" primarykey="order_id" dataNode="ordb" />
	<table name="order_detail" primarykey="order_detail_id" dataNode="ordb" />
	<table name="order_cart" primarykey="cart_id" dataNode="ordb" />
	<table name="order_customer_addr" primarykey="customer_addr_id" dataNode="ordb" />
	<table name="region_info" primarykey="region_id" dataNode="ordb" />
	<table name="shipping_info" primarykey="ship_id" dataNode="ordb" />
	<table name="warehouse_info" primarykey="w_id" dataNode="ordb" />
	<table name="warehouse_product" primarykey="wp_id" dataNode="ordb" />
	
	<table name="product_brand_infos" primarykey="brand_id" dataNode="prodb" />
	<table name="product_category" primarykey="category_id" dataNode="prodb" />
	<table name="product_comment" primarykey="comment_id" dataNode="prodb" />
	<table name="product_info" primarykey="product_id" dataNode="prodb" />
	<table name="product_supplier_info" primarykey="supplier_id" dataNode="prodb" />
	<table name="product_pic_info" primarykey="product_pic_id" dataNode="prodb" />
	<table name="product_brand_infos" primarykey="brand_id" dataNode="prodb" />
	
	<table name="customer_balance_log" primarykey="balance_id" dataNode="custdb" />
	<table name="customer_inf" primarykey="customer_inf_if" dataNode="custdb" />
	<table name="customer_level_inf" primarykey="customer_level" dataNode="custdb" />
	<table name="customer_login" primarykey="customer_id" dataNode="custdb" />
	<table name="customer_login_log" primarykey="login_id" dataNode="custdb" />
	<table name="customer_point_log" primarykey="point_id" dataNode="custdb" />
</schema>

<dataNode name="ordb" dataHost="mysql0103" database="order_db" />
<dataNode name="prodb" dataHost="mysql0104" database="product_db" />
<dataNode name="custdb" dataHost="mysql0105" database="customer_db" />

<dataHost name="mysql0103" maxCon="1000"   minCon="10" balance="3" writetype="0" dbtype="mysql" dbDriver="native" switchtype="1" >

	<hearbeat>select user()</hearbeat>

	<writeHost host="192.168.1.3" url="192.168.1.3:3306" user="im_mycat" password="123456" >

</dataHost>

<dataHost name="mysql0104" maxCon="1000"   minCon="10" balance="3" writetype="0" dbtype="mysql" dbDriver="native" switchtype="1" >

	<hearbeat>select user()</hearbeat>

<writeHost host="192.168.1.4" url="192.168.1.4:3306" user="im_mycat" password="123456" >

</dataHost>


<dataHost name="mysql0105" maxCon="1000"   minCon="10" balance="3" writetype="0" dbtype="mysql" dbDriver="native" switchtype="1" >

	<hearbeat>select user()</hearbeat>

<writeHost host="192.168.1.5" url="192.168.1.5:3306" user="im_mycat" password="123456" >

</dataHost>
~~~
去192.168.1.3创建一个mycat用户
> mysql -uroot -p

> create user im_mycat@'192.168.1.%' identified by '123456';

用户授权
> grant select ,insert,update,delete on *.* to  im_mycat@'192.168.1.%';

查看创建表的语句

> show create table order_detail\G

## mycat配置 vi server.xml

编辑文件
> vi server.xml

~~~
<?xml version="1.0" encoding="UTF-8"?>
<!-- - - Licensed under the Apache License, Version 2.0 (the "License"); 
	- you may not use this file except in compliance with the License. - You 
	may obtain a copy of the License at - - http://www.apache.org/licenses/LICENSE-2.0 
	- - Unless required by applicable law or agreed to in writing, software - 
	distributed under the License is distributed on an "AS IS" BASIS, - WITHOUT 
	WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. - See the 
	License for the specific language governing permissions and - limitations 
	under the License. -->
<!DOCTYPE mycat:server SYSTEM "server.dtd">
<mycat:server xmlns:mycat="http://io.mycat/">
	<system>
		<property name="serverPort">8066</property>
		<property name="managerPort">9066</property>
		<property name="nonePasswordLogin">0</property>
		<property name="bindIp">0.0.0.0</property>
		<property name="frontWriteQueueSize">2048</property>
		
		<property name="charset">utf8</property>
		<property name="txIsolation">2</property>
		<property name="processors">8</property>
		<property name="idleTimeout">1800000</property>
		<property name="sqlExecuteTimeout">300</property>
		<property name="useSqlstat">0</property>
		<property name="useGlobleTableCheck">0</property>
		<property name="sequnceHandlerType">2</property>
		<property name="defaultMaxLimit">100</property>
		<property name="maxPacketSize">104857600</property>
	</system>
	<user name="app_imooc" defaultAccount="true">	
	<property name="usingDecrypt">1</property>
		<property name="password">bDbWr7bVMgszTe82oMo8NaUsmFFdPCNl/lYXzOYoG8anTpQLvdx5e+LYJEmT0IAeSVp1loyxSZPyv1GoHbHFHg==</property>
		<property name="schemas">imooc_db</property>
	</user>

</mycat:server>
~~~

2.需要用到mycat lib下面的java包
>  java -cp  Mycat-server-1.6.5-release.jar io.mycat.util.DecryptUtil 0:app_imooc:123456

3.生成字符串
> bDbWr7bVMgszTe82oMo8NaUsmFFdPCNl/lYXzOYoG8anTpQLvdx5e+LYJEmT0IAeSVp1loyxSZPyv1GoHbHFHg==

4.切换应用通过mycat连接数据库

5.开始测试，由于进行多表连接查询，多个表不在同一个节点中，所以报错
~~~
报错处理：
~~~

## 清理数据

1.进入数据库

> mysql -uroot -p

2.停掉节点的主从同步

> stop slave

3.查看状态

> show slave status\G

4.清除所有的主从同步信息

> set slave all;

5.show slave status \G

> 返回信息：Empty set（0.00 sec）

 同理去停掉节点2 节点3



删除节点2的其他表：

>drop table customer_balance_log;

删除各节点剩下的表。



## 配置全局表

~~~
<table name="region_info" primarykey="region_id" dataNode="ordb,prodb,custdb" type="global" />
~~~

这样就可以多表连接查询了。

## 查看mysql中只读属性

> show varibales like 'read_only';

关闭只读属性

> set global read_only=off

在通过mycat修改数据库，查看数据库中是否都修改了。

## 垂直切分的优点和缺点！

~~~
垂直切分的优点
1.数据库的拆分简单明了
2.应用程序模块清晰明了
3.数据库维护方便易行，容易定位。	
~~~

~~~
垂直切分的缺点
1.部分表关联无法在数据库级别完成，需要在程序中完成
2.对应访问及其频繁且数据量超大的表依然存在性能瓶颈
3.切分达到一定程度之后，扩展性会遇到限制。
~~~
解决：

~~~
解决跨分片关联的方式：
1.使用mycat全局表
2.冗余部分关键数据
3.使用程序api的方式获取数据
~~~



~~~
后续工作：
1.切换应用到mycat
2.删除不属于本模块的表
~~~

