!!! info "提示"
    Android开发框架(其中也包括全平台开发框架)除了`Native`外，还有`Flutter`、`ReactNative`以及本人一直调教不了的`Xamarin Forms`，本人所讲的教程主要围绕安卓原生应用展开。

!!! tip "读者须知"
    该文章所涉及的代码为kotlin[^1]，如果你使用的是Java，不要担心，稍微理解以下就可以了。

## 观察者模式(数据驱动)

### 情景

直接通过**事件**修改UI视图虽然直观，但是随着规模的变大，我们很容易看到处理数据的逻辑和更新UI的逻辑会混用在一起，这样就显得凌乱，而且可能会产生数据和视图更新不一致的问题。我们称处理的模式被称为**事件驱动模式**。

!!! fail "事件驱动模式"
    用户提交 -> 事件触发器 (-> 在UI树上传递事件) -> 更改视图 and 修改数据存储 (->更新本地存储)

我们知道，UI是一种展示数据的界面，所以我们可以这样设计：通过修改数据，然后通知UI层去更改，这种处理的模式叫做**数据驱动模式**

!!! success "数据驱动模式"
    part1: 视图初始化 -> 创建数据的Observer

    part2: 用户提交 -> 修改数据 -> 数据通知更改，并告诉Observer -> Observer更改UI层
    
    数据驱动模式还可以更好的实现异步，在修改数据时使用异步调用，在更新UI时回到UI线程。

### 代码实现

#### 1.安装依赖

打开`build.gradle(Module:app)`，在`dependencies`加入以下两行

```gradle
//viewModel(视图模型) includes(observer<T>)
implementation 'androidx.lifecycle:lifecycle-extensions:2.0.0'
implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.0.0'
```

#### 2.创建存储类

在这里使用全局的`Storage`类，该类的编写如下:

```kotlin
package com.tty.myday.data

import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData

class Storage private constructor() :ILoadable{

    override var isLoaded: Boolean = false

    override fun init() {
        //To init the Storage
        userName.value = "cht"
    }

    override fun load() {
        //To load the Storage
        isLoaded = true
    }

    //region props
    var userName = MutableLiveData<String>()
    //endregion

    companion object {
        private var instance:Storage? = null

        fun get():Storage{
            if (instance == null){
                instance = Storage()
                instance!!.init()
            }

            return instance!!
        }
    }
}
```

#### 3.创建Observer和修改数据

要实现观察者模式需要使用`MutableLiveData<T>`类的对象。1.该对象的`value`属性为内部存储的值，其内部重写了`setter`，使得其可以通知数据已经更改。2.该对象的`observe()`函数为创建观察器。使用该类可以更好的实现数据驱动。

当你创建视图时你应该写以下代码：

```kotlin
//一般在Activity的onCreate()内写
storage = Storage.get()
storage.userName.observe(this,Observer<String>( value -> {
    //dosomething update UI
}))
```

当你需要更改数据时可以编写以下代码

```
storage.userName.value = "xxx"
```

!!! warning "多线程操作"
    更改数据需要在uiThread上执行，否则会发生错误java.lang.IllegalStateException: Cannot invoke setValue on a background thread。

#### 4.整合以上代码，并观察效果。

```kt
// onCreate()
storage = Storage.get()
storage.userName.observe(this,Observer<String>( value -> {
    tv1.text = value
}))
btn1.setOnClickListener{ view -> {
    storage.userName.value = "hello"
}}
```

??? question "思考上述代码的效果"
    你会发现按下按钮之后，tv1显示"Hello"，请思考实现的过程。

## kotlin优雅的异步方式：协程[^2]

### 情景

写了这么多程序的你，还在使用Message+Handler+Looper这种古老的手段的吗，对于异步，你可以更优雅的使用kotlin的协程(Coroutines)。这样你就可以更加方便的写异步了。

### 代码实现

#### 1.安装依赖

打开`build.gradle(Module:app)`，在`dependencies`加入以下一行

```gradle
//kotlin.coroutines(协程)
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.0.0'
```

#### 2.开启异步线程

```kt
import kotlinx.coroutines.*

GlobalScope.launch {
    //dosomething
    runOnUiThread {
        //dosomething
    }
}
```

## 备忘录

### 样式

#### 设置应用为无边框

打开`res\values\styles.xml`在`AppTheme`中加入：

```xml
<item name="windowNoTitle">true</item>
```

#### 设置字体为粗体

```
textStyle="bold"
```

[^1]: kotlin: 由[JetBrains](https://www.jetbrains.com/)公司开发的一门语言。[官方文档](https://kotlinlang.org/) [教程：简书](https://www.jianshu.com/p/f98dcd2da323)

[^2]: 协程：Coroutines，kotlin的一种异步实现方法。[协程概述](http://www.kotlincn.net/docs/reference/coroutines-overview.html)

