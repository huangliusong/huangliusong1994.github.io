# git提交操作代码

+ 1.cd到你的项目目录
> $ cd /Users/mac/Desktop/GitTest  
+ 2.然后,输入git命令:
>git init  
+ 3.将项目的所有文件添加到缓存中:
> git add .  
+ 4.将缓存中的文件Commit到git库
> git commit -m "添加你的注释,一般是一些更改信息"
+ 5.建立远程库
>  git remote add origin HTTPS链接
+ 6.上传代码到远程库,上传之前最好先Pull一下,再执行命令: git pull origin master
> git pull origin master  
> git push -u origin --all
+ 7.接着执行:git push origin master
> git push origin master  
+ 8.使用强制push的方法
> git push -u origin master -f
+ 移除远程连接
> git remote rm origin

+ 查看用户名以及邮箱，使用git config命令

> git config user.email

+ 修改用户名以及邮箱，使用git config 命令的—global参数
>  git config --global user.name "your name"

> git config --global user.email "your email"
