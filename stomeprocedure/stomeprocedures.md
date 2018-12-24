# 存储过程常用文档

## 1.判断存储过程是否存在

~~~
if exists(select * from dbo.sysobjects where id = object_id(N'[dbo].[aaa_hls]') and OBJECTPROPERTY(id, N'IsProcedure') = 1)
	print 'exists'
else
	print 'no exists'
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
use db

SET ANSI_NULLS ON

set ansi_nulls on


set quoted_identifier on
GO
SET QUOTED_IDENTIFIER ON
GO

begin--start cursor
declare @a_a_c_c_ varchar(18)
declare @a_no_c_  varchar(18)
declare @b_n_e_c_ varchar(18)
declare @b_n_c_c_ varchar(18)
declare @d_a_n_c_ varchar(18)
	begin--start define cursor
		declare hls_cursor cursor for 
		select app_code_c,to_account_no_c,brokerage_name_eng_c,brokerage_name_chi_c,disp_account_no_c
		from bh_alert_master
		
		open hls_cursor
		
		fetch next from hls_cursor into 
		@a_a_c_c_,
		@a_no_c_,
		@b_n_e_c_,
		@b_n_c_c_,
		@d_a_n_c_
		while (@@FETCH_STATUS<>-1)
		begin--start cursor while
			if (@a_a_c_c_='G')
				print ' : '+@a_no_c_+' : '+@a_a_c_c_
			else
				print '>>'+@a_no_c_
			fetch next from hls_cursor into 
			@a_a_c_c_,
			@a_no_c_,
			@b_n_e_c_,
			@b_n_c_c_,
			@d_a_n_c_
		end--end cursor while
		DEALLOCATE hls_cursor
	end--end define cursor
end--end cursor



~~~

注意：

> SET ANSI_NULLS  ON

这些是 SQL-92 设置语句，使 SQL Server 2000/2005 遵从 SQL-92 规则。
当 SET QUOTED_IDENTIFIER 为 ON 时，标识符可以由双引号分隔，而文字必须由单引号分隔。当 SET QUOTED_IDENTIFIER 为 OFF 时，标识符不可加引号，且必须符合所有 Transact-SQL 标识符规则。  
SQL-92 标准要求在对空值进行等于 (=) 或不等于 (<>) 比较时取值为 FALSE。当 SET ANSI_NULLS 为 ON 时，即使 column_name 中包含空值，使用 WHERE column_name = NULL 的 SELECT 语句仍返回零行。即使 column_name 中包含非空值，使用 WHERE column_name <> NULL 的 SELECT 语句仍会返回零行。  
当 SET ANSI_NULLS 为 OFF 时，等于 (=) 和不等于 (<>) 比较运算符不遵从 SQL-92 标准。使用 WHERE column_name = NULL 的 SELECT 语句返回 column_name 中包含空值的行。使用 WHERE column_name <> NULL 的 SELECT 语句返回列中包含非空值的行。此外，使用 WHERE column_name <> XYZ_value 的 SELECT 语句返回所有不为 XYZ_value 也不为 NULL 的行。



## 8.ISNULL

~~~
1.isnull(参数1，参数2) 判断参数1 是否为NULL，如果是 返回参数2 否则返回参数1.
  
2.isnull(列名，0)<>0: 先判断 列名是否为null ,然后再与0比较 等于零返回结果为True 否则为False

isnull(value1,value2)

    1、value1与value2的数据类型必须一致。

    2、如果value1的值不为null，结果返回value1。

    3、如果value1为null，结果返回vaule2的值。vaule2是你设定的值。
~~~

## 9.SET NOCOUNT ON;

> SET NOCOUNT ON; 表示关闭输出多少行受影响

