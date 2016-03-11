title: 如何通过Monkey解放双手咻一咻
date: 2016.02.04 23:01
categories: Android-笔记
tags:
- android
- monkey test
- 支付宝
- 咻一咻
---
> 这两天早上10点支付宝咻一咻戳到手机都快破一个洞了还是毛都没有！手还疼得要命，又不甘心错过一丝丝得到敬业福的机会。作为一个程序猿，怎么能让阿里的程序猿耍得团团转呢，于是有了这篇文章。
声明：本文章仅供娱乐及安卓爱好者开发学习交流之用，如果使用该文章中的代码，产生的包括但不局限于手机发热，死机，变砖，电脑罢工，支付宝封号，查水表等均与本人无关 ：） 

#### prepare
1. PC 装好android sdk，配制好环境变量 
2. 准备一台安卓手机，连上电脑，打开USB调试
3. 了解monkey测试的基本知识，不了解的可以看我的这篇[monkey test 使用笔记](http://www.jianshu.com/p/d414c2320536)
* 当然用其它自动化测试也可以，反正只需要简单的模拟点击操作

#### python脚本
1. USB调试-打开指针位置，切到支付宝页面，按住咻一咻，看看手机状态栏上的信息，拿到咻咻咻按钮的坐标
![USB调试-打开指针位置](http://upload-images.jianshu.io/upload_images/1181400-a95b15f4dab67ce8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 编写脚本
新建文本文档，命名touch.py，写入下面这几行代码 

```
import sys
from com.android.monkeyrunner import MonkeyRunner as mr
from com.android.monkeyrunner import MonkeyDevice as md
from com.android.monkeyrunner import MonkeyImage as mi

device = mr.waitForConnection()
if not device:
    print >> sys.stderr,"no device"
    sys.exit(1)
	
for i in range(1,1000000):
	device.touch(584,1052,md.DOWN_AND_UP)	
	mr.sleep(0.2) 
```
  * for 就是循环点了后面的数设大一点，因为目前的情况来看，点个几万下估计不一定有个毛线
  *  touch当然就是点击了，584,1052是刚刚拿到的坐标
  * `mr.sleep(0.2) `表示一秒5点（等0.2秒，当然这个数字可以设大点）。


####  let's go 执行脚本命令
1. 保证手机以正常连接并被电脑识别到可以通过`adb devices`检查
* 打开支付宝咻一咻页面
* 打开cmd然后执行
`C:\Users\Sam>monkeyrunner d:\monkey\touch.py`
配制好SDK环境变量后，monkeyrunner可以在任何目录下运行，但是.py文件必须使用完整路径
* 如果咻到什么东西，如果是没什么用的弹窗只要按返回键就会继续自动咻，如果需要领取，则先停止，只要在命令行界面按`ctrl c`，则会出现停止脚本的提示如下：
![](http://upload-images.jianshu.io/upload_images/1181400-ca7420f35959754f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 如果按终止操作再执行可能会报错，不懂为啥，只需要再终止一次，再执行脚本即可。

#### 有图有真相
![图中可能是由于手机开录屏软件的原因有点卡](http://upload-images.jianshu.io/upload_images/1181400-6915e7725c02fbe5.gif?imageMogr2/auto-orient/strip)



> 虽然是写好了，但是明天就回家了，电脑也懒得带回去，所以其实也用不上，不过算是复习了一下monkey的用法吧，下次有这种坑爹的活动再玩咯。不说了，收拾东西去了 - -。
