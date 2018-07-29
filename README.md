## 欢迎来到黄柳淞的配置！

平时工作中，需要很多的配置，这种配置都已经写在有道云笔记，开源中国的博客，但每次打开软件都会很卡，而且打开博客，很难找到自己的的配置且广告非常多，因此希望自己搭建一个博客系统，把自己经常用到的配置放在这些干净的GitHub页面。

### Markdown

这个所有的配置都是使用Markdown高亮语法，可以更加清晰的展现配置信息！


# 文档目录
## [git提交操作代码](/git/git.md)

```markdown
语法高亮代码块

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
```


### Jekyll 皮肤

您的页面站点将使用您在存储库设置中选择的Jykyl主题的布局和样式。 此主题的名称保存在Jekyll的 _config.yml’配置文件中。

### 技术支持和联系

网页有问题吗？看看我们的[文件](https://help.github.com/categories/github-pages-basics/) 或[联系人支持](https://github.com/contact) 我会帮你解决的。
