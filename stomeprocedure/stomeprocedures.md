# 存储过程常用文档

## 1.判断存储过程是否存在

~~~
if exists(select * from dbo.sysobjects where id = object_id(N'[dbo].[aaa_hls]') and OBJECTPROPERTY(id, N'IsProcedure') = 1)
​	print 'exists'
else
​	print 'no exists'
~~~



## 2.删除存储过程

> ​	drop procedure aaa_hls

## 3.创建存储过程

### 3.1带输入参数的存储过程

~~~


create procedure aaa_hls
@appcode varchar(19)

as

begin

print 'where appcode='+@appcode
select * from tablename m where m.app_code_c=@appcode

end

go
~~~

### 3.2不带参数的存储过程

~~~


create procedure aaa_hls


as

begin


select * from tablename m where m.app_code_c='GTJA'

end

go
~~~



## 4.修改存储过程

~~~
alter procedure aaa_hls
@appcode varchar(19)

as
begin

	print 'where appcode='+@appcode
	select * from bh_master m where m.app_code_c=@appcode

end

go
~~~

## 5.定义变量/赋初值

~~~
declare @hlsname  varchar(12)
set @hlsname='hls1'
if @hlsname='hls'
	print 'hls'
else
	print 'no hls'
~~~

## 6.获取当前日期

> last_upd_date_dt=getdate()

## 7.游标使用

~~~
?
~~~

