title: adb-命令
---
#模拟应用程序被杀掉

* 最简单的方法是在DDMS中点击”Stop Porcess”杀掉你的程序，在你调试程序的时候可以这样做。
* 你可以通过模拟器或者一个Root过的真机来测试实际效果：
按Home按键退出你的程序；
在控制台，敲入如下命令（Windows系统下 WIN + R -> cmd -> 回车）

```
# 查看adb版本
adb version

# 进入adb帮助界面
adb help

# 进入shell环境
adb shell

# 找到该APP的进程ID
adb shell ps 

# 找到你APP的包名 
# Mac/Unix: save some time by using grep: 
adb shell ps | grep your.app.package 
# 按照上述命令操作后，看起来是这样子的:  
# USER PID PPID VSIZE RSS WCHAN PC NAME 
# u0_a198 21997 160 827940 22064 ffffffff 00000000 S your.app.package
# 通过PID将你的APP杀掉 
adb shell kill -9 21997 
# APP现在被杀掉啦 

# 启动adb服务,如果它没启动的话
adb start-server

# 关闭服务
adb kill-server

# 查看所连接的设备以及设备所对应的序列号
adb devices

# 安装app,需要注意的是如果连接了两台设备,则会报错,此时可以添加-s <serialNumber>来处理
adb install -r xxxx.apk

# 卸载app
adb unstall packagename

# 清除应用的数据 
adb shell pm clear packagename

# 连接到指定的ip,这个通常配合wifidebug
adb connect <device-ip-address>


# 查看栈顶Activity,可以用来获取包名
adb shell dumpsys activity top

# 查看所有已安装的应用的包名
adb shell pm list packages -f

# am的状态 Activity Manager State
adb shell dumpsys activity

# 包信息 Package Information
adb shell dumpsys package

# 内存使用情况Memory Usage
adb shell dumpsys meminfo

# Memory Use Over Time
adb shell dumpsys procstats

# Graphics State
adb shell dumpsys gfxinfo

# 从手机复制文件出来
adb pull <remote> <local>

# 向手机发送文件 eg. adb push foo.txt /sdcard/foo.txt
adb push <local> <remote>


# 查看手机CPU,可以看到手机架构(eg.ARMv7) 和几核处理器
adb shell cat /proc/cpuinfo


```
