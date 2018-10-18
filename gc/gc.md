# 可视化GC日志分析工具
1.在线工具：http://gceasy.io/

2.GCViewer

3.打印日志相关参数

~~~
-XX:PrintGCDetails -XX:+PrintGCTimeStamps
-XX:+PrintGCDateStamps -Xloggc:$CATALINA_HOME/logs/gc.log
-XX:+PrintHeapAtGC -XX:+PrintTenuringDistribution
~~~

