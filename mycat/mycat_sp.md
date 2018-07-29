# 数据库水平切分

## 1.水平切分原则

1.能不切分的尽量不要切分

2.选择合适的切分规则和分片键

3.尽量避免跨分片JOIN操作

## 2.水平分片步骤

1.根据业务状态确定要进行水平切分的表

2.分析业务模型选择分片键及分片算法

3.使用mycat部署分片集群

4.测试分片集群

5.业务及数据迁移

 ## 3.如果选择分片键

1.尽可能的比较均匀分布数据到各个节点上

2.该业务字段是最频繁的或者是最重要的查询条件



##  4.分析业务模型选择分片键和分片算法

1.对订单相关表进行水平切分

2.采用简单取模的分片算法



## 5.使用mycat部署分片集群

1.使用schema.xml配置逻辑库和逻辑表

2.使用rule.xml配置分片表的分片规则

3.使用server.xml配置访问用户及权限

## 6.演示环境说明

node1 192.168.1.2 mycat

node2 192.168.1.3 mysql orderdb01 orderdb02

node3 192.168.1.4 mysql orderdb03 orderdb04



## 7.修改mycat配置

> vi schema.xml

~~~
<schema name="imooc_db" checkSQLSchema="false" sqlMaxLimit="100" >

	<table name="order_master" primarykey="order_id" dataNode="orderdb01,orderdb02,orderdb03，orderdb04" />
	
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

<dataNode name="orderdb01" dataHost="mysql0103" database="orderdb01" />
<dataNode name="orderdb02" dataHost="mysql0103" database="orderdb02" />
<dataNode name="orderdb03" dataHost="mysql0104" database="orderdb03" />
<dataNode name="orderdb04" dataHost="mysql0104" database="orderdb04" />

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



### 添加切片
~~~
<dataNode name="orderdb01" dataHost="mysql0103" database="orderdb01" />
<dataNode name="orderdb02" dataHost="mysql0103" database="orderdb02" />
<dataNode name="orderdb03" dataHost="mysql0104" database="orderdb03" />
<dataNode name="orderdb04" dataHost="mysql0104" database="orderdb04" />
~~~
### 添加order_master
~~~
<table name="order_master" primarykey="order_id" dataNode="orderdb01,orderdb02,orderdb03,orderdb04"  rule="order_master"/>
~~~


## 编辑rule.xl

> vi rule.xml

~~~
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mycat:rule SYSTEM "rule.dtd">
<mycat:rule xmlns:mycat="http://io.mycat/">
	<tableRule name="order_master">
	<!--分片规则-->
	<tableRule name="order_master">
		<rule>
			<!--根据id进行分片 运算法则-->
			<columns>customer_id</columms>
			<algorithm>mod-long</algorithm>
		</rule>
	</tableRule>
	
	<function name="mod-long" class="io.mycat.route.function.PartitionByMod">
		<property name="count">4</property>
	</function>
</mycat:rule>
~~~

## 全局自增

~~~
通过mycat全局自增

登录mysql
创建一个数据库mycat
create database mycat;

在mycat的conf目录下有一个dbseq.sql
把这个脚本导入到mycat数据库中
mysql -uroot -p mycat<dbseq.sql

导入完成，然后看一下数据库
usr mycat
show tables;
select * from MY_CAT_SEQUENCE

修改server.xml
<property name="sequnceHandleCheck">0</property>
这个可以死0 1 2 3 4 
0表示以本地文件的方式生成序列号
1表示是数据库的方式
2表示是时间戳的方式
3表示schema方式

修改vi sequence_db_conf.properties
GLOBAL=dn1
COMPANY=dn1
CUSTOMER=dn1
ORDERS=dn1

vi sechema.xml
增加一个数据节点  
<dataHost name="mysql0102" maxCon="1000"   minCon="10" balance="3" writetype="0" dbtype="mysql" dbDriver="native" switchtype="1" >

	<hearbeat>select user()</hearbeat>

	<writeHost host="192.168.1.2" url="192.168.1.2:3306" user="im_mycat" password="123456" >

</dataHost>


增加数据节点
<dataNode name="mycat" dataHost="mysql0102" database="mycat	" />

修改vi sequence_db_conf.properties
GLOBAL=mycat
ORDER_MASTER=mycat



~~~











