## 解决IntelliJ IDEA下日志乱码的问题 [^1]

### server输出乱码

!!! tip "配置环境"
    tomcat：apache-tomcat-9.0.38 <br/>
    IDE：IntelliJ IDEA 2020.2.3 <br/>

#### 方法1：修改logging.propertis

!!! info "原理"
    tomcat输出乱码的原因是tomcat的日志输出一般为utf-8的编码格式的，而日志接收的编码为gbk，因此只要改动其中之一即可。

如图，使用IntelliJ IDEA启动项目时，经常出现日志乱码的问题。

![图片](./../assets/img/web-1.png)

在tomcat的安装目录下`conf\logging.properties`文件中，添加一行`java.util.logging.ConsoleHandler.encoding = GBK`，然后重启服务器。

#### 方法2：修改console的输出编码

在IntelliJ IDEA中点击菜单`File > Setting`，搜索`console`，修改`Default Encoding`为**UTF-8**

![图片](./../assets/img/web-2.png)

### tomcat catalina log输出乱码

!!! info "原理"
    两个日志接受的编码为gbk，因此只要修改`logging.properties`的编码为`GBK`即可（IDEA中没有对应的配置。）

![图片](./../assets/img/web-3.png)

```
1catalina.org.apache.juli.AsyncFileHandler.encoding = GBK

2localhost.org.apache.juli.AsyncFileHandler.encoding = GBK
```

[^1]:参考教程 [教程1](https://blog.csdn.net/gaogzhen/article/details/107307459) [教程2](https://www.cnblogs.com/benchover/p/10773098.html)
