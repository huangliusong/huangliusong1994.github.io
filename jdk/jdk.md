# jdk安装

## 下载jdk

> wget http://7dx.pc6.com/wwb5/jdk7u79linuxx64.tar.gz

## 解压
> tar zxf jdk7u79linuxx64.tar.gz

## 配置java环境
~~~
export PATH=$PATH:/home/mysql/bin:/usr/local/mycat/bin:/usr/local/java/jdk1.7.0_79/bin
export JAVA_HOME=/usr/local/java/jdk1.7.0_79/
export CLASSPATH=.$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export MyCAT_HOME=/usr/local/mycat

~~~

## jdk rpm安装

##下载rpm

## 安装
> 安装jdk rpm
> rpm -ivh jdk文件名
