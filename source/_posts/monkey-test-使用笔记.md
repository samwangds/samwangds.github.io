> Monkey 测试是一种安卓自动化测试，可以用Monkey随机、多次模拟事件来测试应用的负荷程度，并从中获取出错信息，从而优化应用。

* #monkey
monkey的使用比较简单

![monkey 命令](http://upload-images.jianshu.io/upload_images/1181400-303c397ef88d5f1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
`adb shell monkey [options] [event-count]`
options:这个是配置monkey的设置
event-count: monkey执行的事件数

### Monkey命令语法

|参数名|基本功能|示例|
|:-:|:-|:-|
|-p|参数-p用于约束限制，用此参数指定一个或多个包（Package，即App） 。指定包之后，Monkey将只允许系统启动指定的App。如果不指定包，Monkey将允许系统启动设备中的所有APP。|` adb shell monkey -p com.sam.demo 100 `将在Demo包上执行100个随机事件<br> 指定多个包：`adb shell monkey -p com.sam.demo –p com.htc.pdfreader -p com.htc.photo.widgets 100`<br>不指定包：`adb shell monkey 100`：Monkey随机启动APP并发送100个随机事件。|
|-v|	用于指定反馈信息级别（信息级别就是日志的详细程度），总共分3个级别：level0-2|Level 0`adb shell monkey -p com.sam.demo –v 100`  缺省值，仅提供启动提示、测试完成和最终结果等少量信息；<br>Level 1`adb shell monkey -p com.sam.demo –v -v 100`;  提供较为详细的日志，包括每个发送到Activity的事件信息 ;<br> Level 2`adb shell monkey -p com.sam.demo –v -v –v 100` 最详细的日志，包括了测试中选中/未选中的Activity信息|
|-s <seed>	|用于指定伪随机数生成器的seed值，如果seed相同，则两次Monkey测试产生的时间序列也相同。|`adb shell monkey -p com.sam.demo –s 10 100`即使执行两次效果是相同的，因为模拟的用户操作序列（每次操作按照一定的先后顺序所组成的一系列操作，即一个序列）是一样的。操作序列虽然是随机生成的，但是只要我们指定了相同的Seed值，就可以保证两次测试产生的随机操作序列是完全相同的，所以这个操作序列伪随机的.|
|--throttle <毫秒>	|用于指定用户操作（即事件）间的时延，单位是毫秒|`adb shell monkey -p com.sam.demo –-throttle 3000 100`|
|--ignore-crashes	|用于指定当应用程序奔溃时（Force & Close错误），Monkey是否停止运行。如果使用此参数，即使应用程序奔溃，Monkey依然会发送事件，直到事件计数完成。|  `adb shell monkey -p com.sam.demo --ignore-crashes 1000`测试过程中即使程序崩溃，Monkey依然会继续发送事件直到事件数目达到1000为止；<br>`adb shell monkey -p com.sam.demo 1000`测试过程中，如果程序崩溃，Monkey将会停止运行。 |
|--ignore-timeouts	|用于指定当前应用程序发生ANR（Application No Responding）错误时，Monkey是否停止运行，如果使用此参数，即使应用程序发生ANR错误，Monkey依然会发送事件，直到事件计数完成。|类似`--ignore-crashes`|
|--ignore-security-exceptions|	用于指定当程序发生许可错误时（如证书许可，网络许可等），Monkey是否停止运行。如果使用此参数，即使应用程序发送许可错误，Monkey依然会发送事件，直到事件计数完成|类似`--ignore-crashes`|
|--kill-process-after-error	|用于指定应用程序发生错误时，是否停止其运行。如果指定此参数，当应用程序发生错误时，应用程序停止运行并保持在当前状态（注意：应用程序仅是静止在发生错误时的状态，系统并不会结束该应用程序的进程）。|类似`--ignore-crashes`|
|--monitor-native-crashes	|用于指定是否监视并报告应用程序发生崩溃的本地代码|类似`--ignore-crashes`|
|--pct- {+事件类别} {+事件类别百分比}	|用于指定每种类别事件的数目百分比（在Monkey事件序列中，该类事件数据占总事件数目的百分比）|以下几个|
|--pct-touch {+百分比}	|调整触摸时间的百分比（触摸事件是一个down-up事件，它发生在屏幕上的某个单一位置）|`adb shell monkey -p com.sam.demo --pct-touch 10 1000`|
|--pct-motion	|调整动作事件的百分比（动作事件由屏幕上某处的一个down事件、一系列的伪随机事件和一个up事件组成）|`adb shell monkey -p com.sam.demo --pct-motion 20 1000`|
|--pct-trackball {+百分比 }	|调整轨迹事件的百分比（轨迹事件由一个或者几个随机的移动组成，有时还伴随有点击)|`adb shell monkey -p com.sam.demo --pct-trackball 30 1000`|
|--pct-nav	|调整“基本”导航事件的百分比（导航事件由来自方向输入设备up/down/left/right组成）|`adb shell monkey -p com.sam.demo --pct-nav 40 1000`|
|--pct-majornav {+百分比}|	调整“主要”导航事件的百分比（这些导航事件通常引发图形界面中的动作，如：5-way键盘的中间按键、回退按键、菜单按键）|`adb shell monkey -p com.sam.demo --pct-majornav 50 1000`|
|--pct-syskeys {+百分比}	|调整“系统”按键事件的百分比（这些按键通常被保留，由系统使用，如Home、Back、Start Call、End Call及音量|`adb shell monkey -p com.sam.demo --pct-syskeys 60 1000`|
|--pct-appswitch {+百分比}	|调整启动Activity的百分比。在随机间隔里，Monkey将执行一个startActivity（）调用，作为最大程度覆盖包中全部Activity|`adb shell monkey -p com.sam.demo --pct-appswitch 70 1000`|
|--pct-anyevent {+百分比}	|调整其它类型事件的百分比。它包罗了所有其它类型的事件，如：按键、其它不正常的设备按钮，等等|指定单个类型事件的百分比：`adb shell monkey -p com.sam.demo --pct -anyevent 100 1000` <br>指定多个类型事件的百分比：`adb shell monkey -p com.sam.demo --pct-anyevent 50 --pct-appswitch 50 1000`<br>注意：各事件类型的百分比总数不能超过100%|
|--wait-dbg|	停止执行中的Monkey，直到有调试器和它相连接。|  略|

>以上表格数据来源 http://www.jianshu.com/p/bc59b36bb438
好像没什么卵用，基本上只要` adb shell monkey -p com.sam.demo -v 100 `这句就够用了

### 日志解析
1. :Monkey: seed=0 count=100
:AllowPackage: com.example.myspinner
:IncludeCategory: android.intent.category.LAUNCHER
:IncludeCategory: android.intent.category.MONKEY
seed对应其语法中的第一项（伪随机seed值），count表示执行次数；AllowPackage表示测试的包名；IncludeCategory两个是策略。

* Seeded: 0** //种子为0**

* Event percentages:** //事件百分比**
>       0：触摸事件百分比，即参数--pct-touch
        1：滑动事件百分比，即参数--pct-motion
        2：缩放事件百分比，即参数--pct-pinchzoom
        3：轨迹球事件百分比，即参数--pct-trackball
        4：屏幕旋转事件百分比
        5：基本导航事件百分比，即参数--pct-nav
        6：主要导航事件百分比，即参数--pct-majornav
        8：Activity启动事件百分比，即参数--pct-appswitch
        9：键盘翻转事件百分比，即参数--pct-flip
        10：其他事件百分比，即参数--pct-anyevent

*  :SendKey (ACTION_DOWN): 59    // KEYCODE_SHIFT_LEFT
:SendKey (ACTION_UP): 59
SendKey表示发送的动作（action），ACTION_DOWN表示鼠标按下，ACTION_UP表示鼠标抬起。//后面注释的是[按键类型 ](http://www.doc88.com/p-2354397046610.html)

* :Sending Pointer ACTION_DOWN x=234.0 y=357.0 
触摸事件，x、y指定触摸的坐标。

* Events injected: 100
事件注入成功总数。

* :Sending rotation degree=0, persist=false
发送生屏幕翻转 度=0，存留=否

* :Dropped: keys=0 pointers=3 trackballs=0 flips=0 rotations=0
丢失情况，按键事件丢失为0，触摸事件丢弃3种，轨迹球事件丢失为0，键盘轻弹丢失0，键盘翻转事件丢失为0。

* Network stats: elapsed time=3800ms (0ms mobile, 0ms wifi, 3800ms not connected）
网络状态：占用时间=3800ms（手机0ms，wifi 0ms，未连接3800ms） 

* 使用命令` adb shell monkey -p com.sam.demo -v 100 >log.txt` 后面的`>log.txt`可以将日志保存到当前目录下的log.txt文件

***
随机事件使用结束，好像只能压测用，因为没办法控制，下面介绍配合python脚本进行测试
***

* #monkeyRunner
>monkeyrunner支持,自己编写插件,控制事件,随时截图,简而言之,任何你在模拟器/设备中能干的事情,MonkeyRunner都能干,而且还可以记录和回放!

### 首先千方百计得到一个python脚本
自己写或者网上下载都行,注意!如果脚本文件有使用中文,记得格式保存为utf8,不然会编码格式无法支持错误

```
#导入我们需要用到的包和类并且起别名
import sys
from com.android.monkeyrunner import MonkeyRunner as mr
from com.android.monkeyrunner import MonkeyDevice as md
from com.android.monkeyrunner import MonkeyImage as mi
 
#connect device 连接设备
#第一个参数为等待连接设备时间
#第二个参数为具体连接的设备
device = mr.waitForConnection()
if not device:
    print >> sys.stderr,"fail"
    sys.exit(1)
#定义要启动的Activity
componentName='com.sam.sometime/com.sam.sometime.activity.MainActivity'
#启动特定的Activity
device.startActivity(component=componentName)
mr.sleep(3.0)
#do someting 进行我们的操作
#输入 a s d
device.type('asd')
#输入回车
device.press('KEYCODE_ENTER')
#return keyboard 点击返回用于取消等下看到截图的下方的白条
#device.press('KEYCODE_BACK')
#------
#takeSnapshot截图
mr.sleep(3.0)
result = device.takeSnapshot()
 
#save to file 保存到文件
result.writeToFile('D:\\monkeytest\\takeSnapshot\\result1.png','png');
```
### 运行 `monkeyrunner   program.py` 注意py文件使用完整路径
运行成功后就可以在看到 result1这张图片了。

### 一些操作的API
```
1. #导入模块;
    from com.android.monkeyrunner import MonkeyRunner, MonkeyDevice, MonkeyImage

2. #连接当前设备,并返回一个MonkeyDevice对象;
    device = MonkeyRunner.waitForConnection()
    if not device:
        print "Please connect a device to start!"
    else:
        print "Start "
    
3. #安装Android包，注意，此方法返回的返回值为boolean，由此可以判断安装过程是否正常 ;
    device.installPackage('myproject/bin/MyApplication.apk') 

4. #启动一个Activity; 
   device.startActivity(component='com.android.htccontacts/com.android.htccontacts.ContactsTabActivity')

5. #截图;
    result = device.takeSnapshot()
    result.writeToFile('C:\\Users\\Martin\\Desktop\\test.png','png')

6. #时延(秒);
    MonkeyRunner.sleep(3)

7. #滑动屏幕;
    for i in range(1,70):
        device.drag((250,850),(250,110),0.1,10)
    for i in range(1,70):
       device.drag((250,110),(250,850),0.1,10)
   MonkeyRunner.sleep(1)

8. #触击屏幕;
    device.touch(507,72,"DOWN_AND_UP")

9. #执行adb shell命令;
    device.shell("input text goup01")

10. #长按的实现 
device（100,100，MonkeyDevice.DOWN）
　　MonkeyRunner.sleep(1)
　　device（100,100，MonkeyDevice.UP）
　　当然，也可以通过drag方法实现：device.drag((100,100),(100,100),1,10)

11. 使用"DOWN_AND_UP"如果可以换成MonkeyDevice.DOWN_AND_UP
```

### 网上有通过 `MonkeyRecorder`进行操作的脚本录制有回放，但是我觉得太卡了，不好用，有兴趣的自行百度
