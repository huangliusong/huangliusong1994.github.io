# CMD常用命令



## 1.查看端口占用

>netstat -ano

## 2.查看具体某个端口的占用

> netstat -aon|findstr "49157"

## 3.强制关闭任务

> taskkill /f /t /in java.exe

## 4.查看pid的任务名称

> tasklist|findstr "2720"



