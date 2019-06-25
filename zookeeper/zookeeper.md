# zookeeper使用
## 1.下载zookeeper
[zookeeper-3.4.10](http://mirror.bit.edu.cn/apache/zookeeper/)
## 2.修改config
> zookeeper-3.4.10/conf/zoo-sample.cfg修改为zoo.cfg

## 3.启动
> ./zkServer.sh start

## ZooKeeper服务命令:

### 在准备好相应的配置之后，可以直接通过zkServer.sh 这个脚本进行服务的相关操作

>  启动ZK服务:       sh bin/zkServer.sh start

> 查看ZK服务状态: sh bin/zkServer.sh status

> 停止ZK服务:       sh bin/zkServer.sh stop

> 重启ZK服务:       sh bin/zkServer.sh restart

## zk客户端命令

ZooKeeper命令行工具类似于[Linux](http://lib.csdn.net/base/linux)的shell环境，不过功能肯定不及shell啦，但是使用它我们可以简单的对ZooKeeper进行访问，数据创建，数据修改等操作.  使用 zkCli.sh -server 127.0.0.1:2181 连接到 ZooKeeper 服务，连接成功后，系统会输出 ZooKeeper 的相关环境以及配置信息

> 显示根目录下、文件： ls / 使用 ls 命令来查看当前 ZooKeeper 中所包含的内容

>显示根目录下、文件： ls2 / 查看当前节点数据并能看到更新次数等数据

> 创建文件，并设置初始内容： create /zk "test" 创建一个新的 znode节点“ zk ”以及与它关联的字符串

> 获取文件内容： get /zk 确认 znode 是否包含我们所创建的字符串

> 修改文件内容： set /zk "zkbak" 对 zk 所关联的字符串进行设置

> 删除文件： delete /zk 将刚才创建的 znode 删除

> 退出客户端： quit

> 帮助命令： help

## ZooKeeper 常用四字命令

ZooKeeper 支持某些特定的四字命令字母与其的交互。它们大多是查询命令，用来获取 ZooKeeper 服务的当前状态及相关信息。用户在客户端可以通过 telnet 或 nc 向 ZooKeeper 提交相应的命令

> 可以通过命令：echo stat|nc 127.0.0.1 2181 来查看哪个节点被选择作为follower或者leader
> 使用echo ruok|nc 127.0.0.1 2181 测试是否启动了该Server，若回复imok表示已经启动。

> echo dump| nc 127.0.0.1 2181 ,列出未经处理的会话和临时节点。

> echo kill | nc 127.0.0.1 2181 ,关掉server

> echo conf | nc 127.0.0.1 2181 ,输出相关服务配置的详细信息。

> echo cons | nc 127.0.0.1 2181 ,列出所有连接到服务器的客户端的完全的连接 / 会话的详细信息。

>  echo envi |nc 127.0.0.1 2181 ,输出关于服务环境的详细信息（区别于 conf 命令）。

> echo reqs | nc 127.0.0.1 2181 ,列出未经处理的请求。

> echo wchs | nc 127.0.0.1 2181 ,列出服务器 watch 的详细信息。

> echo wchc | nc 127.0.0.1 2181 ,通过 session 列出服务器 watch 的详细信息，它的输出是一个与 watch 相关的会话的列表。

> echo wchp | nc 127.0.0.1 2181 ,通过路径列出服务器 watch 的详细信息。它输出一个与 session 相关的路径。