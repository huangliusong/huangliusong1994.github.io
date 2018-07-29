# jdk+mysql+tomcat
> wget http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm?AuthParam=1526908418_c5207a32cd063ea0a7f11cf340f4bb9e

> rpm -ivh 文件名

> wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm

> yum -y install mysql57-community-release-el7-10.noarch.rpm
yum -y install mysql-community-server

> systemctl start  mysqld.service

> systemctl status mysqld.service

> grep "password" /var/log/mysqld.log

>  mysql -uroot -p
 
> set global validate_password_policy=0;
 
> set global validate_password_length=1;

>  ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';

> flush privileges;

> grant all privileges  on *.* to root@'%' identified by "root";

> select host,user,password from user;

> wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/apache-tomcat-8.5.31.tar.gz


