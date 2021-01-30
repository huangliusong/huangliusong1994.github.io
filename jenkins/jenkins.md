# jenkins安装
~~~
1.下载
wget http://mirrors.jenkins-ci.org/redhat/jenkins-2.20-1.1.noarch.rpm

2.安装
rpm -ivh jenkins-2.200-1.1.noarch.rpm 

3.安装java
yum install java

4.启动
systemctl start jenkins.service

5.查看密码
cat /var/lib/jenkins/secrets/initialAdminPassword
~~~
