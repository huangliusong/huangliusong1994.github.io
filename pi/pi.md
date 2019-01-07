# 树莓派



## 1.树莓派引脚对照表
![树莓引脚](/images/20181108083724411.png)

# centos安装homebridge


## 2.更新linux安装源
> yum update
## 3.查看
> ls
## 4 
>  curl --silent --location https://rpm.nodesource.com/setup_5.x | bash -
## 5.安装node.js 
>  yum install -y nodejs
## 6.安装homebridge（如果出错，执行7，8，9）
>  sudo npm install -g --unsafe-perm homebridge
## 7.安装gcc编译
>  yum -y install gcc automake autoconf libtool make
## 8.安装c++编译
>  yum install gcc gcc-c++
## 9.安装homebridge
>  sudo npm install -g --unsafe-perm homebridge
## 10. 执行homebridge出现二维码
> homebridge
## 11.
>  cd ~/.homebridge/
## 12.加个config
>  vi config.json
## 13.
>  homebridge
## 14.look it
![执行homebridge出现二维码](/images/WechatIMG43.png)
