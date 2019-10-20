# Kotlin Web[^1]

## 搭建开发环境

### 1.创建一个Kotlin项目

打开Jetbrains IntelliJ IDEA软件，创建Kotlin项目，即先选择{==File > New > New Project...==}，然后选择{==Kotlin > JVM | IDEA==}

更改项目的名称，此教程以{==Demo==}为例，并选择项目的地址

### 2.添加框架支持

右键项目目录，点击{==Add Framework Support==}，然后勾选{==Web Application==}（取消勾选该目录下的{==Create web.xml==}）和{==Maven==}。

### 3.导入缺少的包

!!! tip "需要下载Tomcat"
    缺少的包依赖{==Tomcat==}运行环境，[点击链接](http://tomcat.apache.org/)下载合适的版本

在项目的根目录创建一个`lib`的文件夹，将下载的`tar.gz`解压，将`lib`文件夹中`jsp-api`和`servlet-api`移到刚刚创建的`lib`目录下。

点击{==File > Project Structure...==}选择{==Project Settings==}的{==Libraries==}子项，在{==KolinJavaRuntime==}的右面选择`+`号，然后选中创建的`lib`文件夹。然后确认更改。

选择{==Artifacts==}，然后一直双击{==Available Elements==}的子项，把所有可用的元素全部导入。

### 4.配置启动器

回到代码编辑界面，点击{==Add Configuration...==}，然后点击`+`号，找到{==Tomcat Server> Local==}(在more items里)，切换到{==Deployment==}目录，选择右边的`+`号，选择{==Artifists==}，修改下面的{==Application context==}为合适的路径(本例子以`server`举例)。然后确认更改。

运行，查看效果，如果你看到浏览器弹出一个页面，则表示配置成功。

!!! note "提示"
    如果看到端口被占用的情况，可以关闭其他已经开启的服务器。

### 5.添加页面

在`src\main\java\`创建`Api.kt`，然后修改内容如下

```kt
package main

import javax.servlet.annotation.WebServlet
import javax.servlet.http.HttpServlet
import javax.servlet.http.HttpServletRequest
import javax.servlet.http.HttpServletResponse

@WebServlet(name="api", urlPatterns = ["/api"])
class Api:HttpServlet() {
    override fun doGet(req: HttpServletRequest?, resp: HttpServletResponse?) {
        resp?.writer?.write("hello")
    }
    override fun doPost(req: HttpServletRequest?, resp: HttpServletResponse?) {
        
    }
}
```

运行，输入`./server/api`查看结果，如果看到`hello`，就表示运行成功了。

[^1]: 此文根据[教程](https://blog.csdn.net/qq_33243443/article/details/72895806)编写
