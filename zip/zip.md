# 解压文件

## 两种解压方式

>  tar -zxvf java.tar.gz

解压到指定的文件夹

>   tar -zxvf java.tar.gz  -C /usr/java

## **gz**文件的解压 gzip 命令

>  gzip -b java.gz

也可使用**zcat** 命令，然后将标准输出 保存文件

> zcat java.gz > java.java