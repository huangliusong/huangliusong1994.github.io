# hexo安装

## 依次执行下面命令
~~~
npm install hexo-cli -g
npm install hexo --save
//如果上面的命令由于网络原因，安装失败，那么可以切换一下npm源，执行一下下面命令

npm install -g cnpm --registry=https://registry.npm.taobao.org
//然后执行
cnpm install hexo --save
~~~

## 然后执行
~~~
//然后执行
hexo init
//然后装一下hexo的一些插件，建议全部安装
npm install hexo-generator-index --save #索引生成器
npm install hexo-generator-archive --save #归档生成器
npm install hexo-generator-category --save #分类生成器
npm install hexo-generator-tag --save #标签生成器
npm install hexo-server --save #本地服务
npm install hexo-deployer-git --save #hexo通过git发布（必装）
npm install hexo-renderer-marked@0.2.7--save #渲染器
npm install hexo-renderer-stylus@0.3.0 --save #渲染器
~~~

## 安装完成之后跑起来试试
~~~
运行 hexo server 或者 hexo s
~~~

## HEXO配置
### 全局配置文件_config.yml
~~~
index_generator:
  per_page: 10 ##首页默认10篇文章标题 如果值为0不分页
archive_generator:
    per_page: 10 ##归档页面默认10篇文章标题
    yearly: true  ##生成年视图
    monthly: true ##生成月视图
tag_generator:
    per_page: 10 ##标签分类页面默认10篇文章
category_generator: 
    per_page: 10 ###分类页面默认10篇文章
feed:
    type: atom ##feed类型 atom或者rss2
    path: atom.xml ##feed路径
    limit: 20  ##feed文章最小数量
deploy:
    type: git ##部署类型 其他类型自行google之
    repo: <repository url> ##git仓库地址
    branch: [branch] ##git 页面分支
    message: [message] ##git message建议默认字段update 可以自定义
    -多部署
            deploy:
                    type: git
                    message: update  ##git message建议默认字段update 可以自定义
                    repo: 
                    github: <repository url>,[branch] ##github 仓库地址和分支
                    gitcafe: <repository url>,[branch] ##gitcafe 仓库地址和分支
~~~


## 主题配置文件_config.yml
~~~
# Header
menu:   #导航栏连接
Home: /
Archives: /archives #归档页面URL
自定义页面标题: /自定义页面URL
rss: /atom.xml  #rss地址  默认即可
# Content
excerpt_link: Read More #阅读更多的文字显示
fancybox: true #开启fancybox效果
# Sidebar  #侧边栏设置
sidebar: right
widgets:
    - category
    - tag
    - tagcloud
    - archive
    - recent_posts
# Miscellaneous #社交网络和统计连接地址
google_analytics: #google analytics ID
favicon: /favicon.png #网站的favicon
twitter:
google_plus:
fb_admins:
fb_app_id:
~~~

## 配置主题
~~~
这里介绍我自己修改的主题，炒鸡漂亮

基于landscape-plus主题修改 ，GIT地址

演示地址

使用方法，下载下来丢到D:\testblog\themes 文件夹下面就好了

然后修改全局配置文件_config.yml文件里面theme字段
~~~

## 配置GIT发布
~~~
deploy: 
    type: git   
    repo: https://github.com/mustfun/mustfun.github.com.git
     #我的github是mustfun 换成你本人的
    branch: master
~~~

## HEXO部署
~~~
hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署
~~~


https://hexo.io/zh-cn/docs/

hexo server --port=4001


# docker-hexo

## create hexo container,just run:

> docker run  -d --name some-hexo -p 4000:4000  ipple1986/hexo

## enter the hexo container ,just run:
> docker exec -it some-hexo bash

## visit the Hexo website
> just visit http://127.0.0.1:4000 through your web browser. or curl http://127.0.0.1:4000

## other configuration
~~~
default port 4000
hexo server directory is  /opt/hexo/ipple1986
~~~