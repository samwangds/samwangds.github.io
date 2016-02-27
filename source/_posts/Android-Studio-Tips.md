title: Android-Studio-Tips
---
>收集网上看到，或者自己总结的tips，未完待续，持续更新 
文中所提到的快捷键无特殊说明均为windows环境下，如果遇到快捷键无效，检查是否被占用，或者设置中，keymap相关键位是否设置
[兄弟篇 - android 开发tips - sdk api](http://www.jianshu.com/p/adfa519756b1)


1. 在常量(如：1，"XXX")后输入`.var`回车可快速生成临时变量，输入`.field` 回车可快速生成全局变量 

1. 选中后可以使用 Extra 快捷键重构为变量、方法等，这个可以在 Refactor -> Extra 下看到。
ctrl+ alt+ v：变量
ctrl+ alt+ c：常量
ctrl+ alt+ f：域值
ctrl+ alt+ p：参数
ctrl+ alt+ m：方法
ctrl+ alt+ R：重命名
1. 在可以循环遍历的变量后输入`.for` 或者`.fori .forr` 回车可快速遍历该对象
 
1. android studio 设置Keymap里面的`Fix doc comment`快捷键名，可以快速生成注释，点击变量名或者方法名，再按快捷键即可使用
![android studio setting 截图](http://upload-images.jianshu.io/upload_images/1181400-053142c8e142011e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
1. `live templates` 让你在android studio风一样的写代码 [Github上的一个项目，整理了许多有用的模版，可以直接导入AndroidStudio使用]( https://github.com/keyboardsurfer/idea-live-templates)
将下载下来的xml文件拷贝到下面的路径，如果不存在文件夹则新建，并重启android studio
```
Live templates are stored in the following location:
Windows: <your home directory>\.<product name><version number>\config\templates
Linux: ~/.<product name><version number>/config/templates
OS X: ~/Library/Preferences/<product name><version number>/templates
```

* 多行编辑 使用**Alt+鼠标左键**（按住alt同时点左键拖动），或者选中代码，然后使用快捷键**Shift+Alt+Insert﻿**
 
* 选中代码，右键，与剪切板中的代码比较 
![](https://camo.githubusercontent.com/98f603e75700d5099ab27205bdf8fc065c272ab2/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f2d3672446e386b4c375067772f56436c454d31336f594b492f41414141414141414e306f2f4a576964755731705773552f773531392d683236352d6e6f2f33342d636f6d7061726577697468636c6970626f6172642e676966)

* 跳到下一行，即使不在行尾的时候 **Ctrl-Shift-Enter﻿**
 
* 设置全局参数，在Project的`build.gradle`中加入
```
ext {//定义project公用参数，在Modules 使用rootProject.ext.XX就能拿到相应对象
    compileSdkVersion = 23
    buildToolsVersion = "23.0.1"
    supportLibVersion = "23.1.1"
...
}
```
在Module的`build.gradle`中把引用改为
```
android {   
     compileSdkVersion rootProject.ext.compileSdkVersion
     buildToolsVersion rootProject.ext.buildToolsVersion
     ...
}
dependencies { 
    compile "com.android.support:appcompat-v7:${supportLibVersion}"
    compile "com.android.support:design:${supportLibVersion}" 
}
```
此时如果参数进行修改则只需要更改Project的`build.gradle`更加方便和统一

* 参数信息（Parameter Info）
描述：这个操作将显示和你在方法声明处写一样的参数列表，当你想看某个存在的方法的参数，这是一个很有用的操作。光标下的参数显示为黄色，如果没有参数显示黄色，意味着你的方法调用是无效的，很可能是某个参数分配不对。（例如一个浮点数赋值给了整型参数）。如果你正在写一个方法调用，突然离开编辑的地方，再返回的时候，输入一个逗号，就可以重新触发参数信息。
快捷键：`Cmd + P(OS X)、Ctrl +U (Windows/Linux)`
![](http://upload-images.jianshu.io/upload_images/1181400-b56de291add6bb32?imageMogr2/auto-orient/strip)

* 快速查看定义（Quick Definition Lookup）
描述：在当前界面查看一个方法或者类的具体实现 
快捷键：`
Alt + Space / Cmd + Y(OS X)、Ctrl + Shift + I(Windows/Linux)`
![](http://upload-images.jianshu.io/upload_images/1181400-638260362d0bc43d?imageMogr2/auto-orient/strip)
 
*  相关文件（Related File）
描述：该操作有助于在布局文件和Activity/Fragment之间轻松跳转。这也是一个快捷操作，在类名/布局顶端的左侧。
快捷键：`
Ctrl + Cmd + Up(OS X)、Ctrl + Alt + (Windows/Linux)`
![](http://upload-images.jianshu.io/upload_images/1181400-65cf75af3a8d2059?imageMogr2/auto-orient/strip)

* 包裹代码（Surround With）
描述： 该操作可以用特定代码结构包裹住选中的代码块，通常是if语句，循环，try/catch语句或者runnable语句。 
如果你没有选中任何东西，该操作会包裹当前一整行。
快捷键：`
Cmd + Alt + T(OS X)、Ctrl + Alt + T(Windows/Linux)`
![](http://upload-images.jianshu.io/upload_images/1181400-1094ce6c09a435fe?imageMogr2/auto-orient/strip)

49. 移除包裹代码（Unwrap Remove）
描述：该操作会移除周围的代码，它可能是一条if语句，一个while循环，一个try/catch语句甚至是一个runnable语句。该操作恰恰和包裹代码（Surround With）相反。
快捷键：`Cmd + Shift + Delete(OS X)、Ctrl + Shift + Delete(Windows/Linux)`
![](http://upload-images.jianshu.io/upload_images/1181400-24f01ac84b4b82dd?imageMogr2/auto-orient/strip)

* 调用层级树弹窗（The Call Hierarchy Popup）
描述：该操作会给你展示 在一个方法的声明和调用之间所有可能的路径。
快捷键：`Ctrl + Alt + H`

* 利用Gradle删除没有使用到的资源文件
在gradle中配置shrinkResources true，同时 minifyEnabled也要为true才行。

---
### DEBUG 相关
1. debug的断点设置在循环里面，可以通过右键断点，来设置进入的条件 
![](https://camo.githubusercontent.com/c7529147fed09597ed30b559e560ae9e997f9987/68747470733a2f2f6c68362e676f6f676c6575736572636f6e74656e742e636f6d2f2d70396b364a694e4c516d592f5642417765666c726b59492f41414141414141414e58382f67436175666a47626431632f773531342d683236342d6e6f2f32322d636f6e646974696f6e616c627265616b706f696e742e676966)

1. 日志断点（Logging Breakpoints）
 这是一种打印日志而不是暂停的断点，当你想打印一些日志信息但是不想添加log代码后重新部署项目，这是一个非常有用的操作。
调用：在断点上右键，取消`Suspend`的勾选，然后勾选上`Log evaluated Expression`，并在输入框中输入你要打印的日志信息。

*  显示当前运行点（Show Execution Point）
该操作会立刻把你的光标移回到当前debug处。
快捷键：（Debug时) `Alt + F10`

* 临时断点（Temporary Breakpoints）
描述：通过该操作可以添加一个断点，这个断点会在第一次被命中的时候自动移除。
快捷键：Alt + 鼠标左键 点击代码左侧（鼠标）、Cmd + Alt + 
Shift + F8(OS X)、Ctrl + Alt + Shift + F8(Windows/Linux)


to be continue...
