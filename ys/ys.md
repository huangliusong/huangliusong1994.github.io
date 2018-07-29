# CentOS7 搭建影梭服务器

## 1.安装Python包管理工具
> yum install python-setuptools && easy_install pip 

## 2.安装Shadowsocks
> pip install shadowsocks

## 3.启动服务

### 3.1命令配置运行

> ssserver -p 443 -k password -m aes-256-cfb  // ssserver -p 服务器端口 -k 密码 -m 加密方法

> ssserver -p 443 -k password -m aes-256-cfb -d start // -d start //代表后台运行

### 3.2配置文件运行 

#### 3.2.1创建/etc/shadowsocks/目录

> mkdir /etc/shadowsocks

#### 3.2.3在/etc/shadowsocks/目录下创建配置文件

> vim /etc/shadowsocks/conf.json

#### 3.2.3.1单用户配置
~~~
{
 "server":"0.0.0.0",     // 你的服务器ip
 "server_port":8388,             // 端口号（每一个账号都不能重复）
 "local_address": "127.0.0.1",  // 本地地址，一般不变
 "local_port":1080,             // 本地端口，一般不变
 "password":"*********",            // 连接密码
 "timeout":300,                 // 相应超时时间
 "method":"aes-256-cfb",            // 加密方式
 "fast_open": false             //  使用TCP_FASTOPEN, 参数选项true / false，一般保持默认即可
}
~~~

#### 3.2.3.2多用户配置
~~~
{
 "server":"0.0.0.0",
 "local_address": "127.0.0.1",
 "local_port":1080,
 "port_password":{
      "9999":"huangliusong",
      "9001":"password1",
      "9002":"password2",
      "9003":"password3",
      "9004":"password4"
 },
 "timeout":300,
 "method":"aes-256-cfb",
 "fast_open": false
}
~~~
### 根据配置文件启动
~~~
ssserver -c /etc/shadowsocks/conf.json start // 前台运行
ssserver -c /etc/shadowsocks/conf.json -d start // 后台运行
ssserver -c /etc/shadowsocks.json -d stop // 停止服务
~~~

----
# 错误
~~~
1、配置文件中不允许中文
2、ip修改为0.0.0.0而不是公网ip地址
3.https://blog.sky-city.me/node/59
4.http://blog.csdn.net/hksangs/article/details/53397287
5.https://github.com/shinvdu/shadowsock
6.apk:http://blog.sky-city.me/files/shadowsocks.apk
http://www.xyzbeta.com/66
~~~
# 下载地址
~~~
Step 1. Install Shadowsocks GUI application
Android users: install from Google Play: Shadowsocks-Android
地址：https://play.google.com/store/apps/details?id=com.github.shadowsocks

iOS users: install from App Store: Shadowsocks-iOS [Help: Link]
地址：https://itunes.apple.com/us/app/shadowsocks/id665729974?ls=1&mt=8

For Windows 7 or earlier, download shadowsocks-win-2.3.zip
地址：https://kiwivm.64clouds.com/dist/shadowsocks-win-2.3.zip

For Windows 8 or later, download shadowsocks-win-dotnet4.0-2.3.zip
地址：https://kiwivm.64clouds.com/dist/shadowsocks-win-dotnet4.0-2.3.zip


Alternatively, you may download the latest version from developer's website: http://sourceforge.net/projects/shadowsocksgui/files/dist/

Once downloaded, extract the .zip file and launch Shadowsocks.exe

If you do not see the settings window as shown below, then locate and doubleclick Shadowsocks tray icon:  
~~~
