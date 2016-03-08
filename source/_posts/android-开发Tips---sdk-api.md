title: android-开发Tips---sdk-api
date: 2015.11.20 19:04
categories: Android-笔记
---
收集网上看到，或者自己总结的tips 
1. `view.isShown ()` 当view本身以及它的所有祖先们都是visible时，isShown（）才返回TRUE。
而平常我们调用`if(view.getVisibility() == View.VISIBLE)`只是对view本身而不对祖先的可见性进行判断

1. `android.text.format.Formatter` 方法 ：`public static String formatFileSize(Context context, long number)` 
number 的单位是B，返回如：3.33MB  (B->KB->MB->GB->TB->PB)
```
<!-- Suffix added to a number to signify size in bytes. -->
<string name="byteShort">B</string>
<!-- Suffix added to a number to signify size in kilobytes. -->
<string name="kilobyteShort">KB</string>
<!-- Suffix added to a number to signify size in megabytes. -->
<string name="megabyteShort">MB</string>
<!-- Suffix added to a number to signify size in gigabytes. -->
<string name="gigabyteShort">GB</string>
<!-- Suffix added to a number to signify size in terabytes. -->
<string name="terabyteShort">TB</string>
<!-- Suffix added to a number to signify size in petabytes. -->
<string name="petabyteShort">PB</string>
```

1. `android.media.ThumbnailUtils`类，用来获取媒体（图片、视频）缩略图
1. `String.format(String format, Object... args)` 与 `Context.getString(int resId, Object... formatArgs)`用于格式化strings.xml中的字符串
1. `TextView.append(CharSequence text)` 和 `append(CharSequence text, int start, int end)`方法，直接在Textview后面追加字符串
1. `DecimalFormat`类，用于字串格式化包括指定位数、百分数、科学计数法等
1. `android.util.Pair`类，封装了两个对象的类(用处自己想吧)
```
public Pair(F first, S second) {    
       this.first = first;    
       this.second = second;
}
```
1. 安卓的单元测试类 `ApplicationTestCase<Application> `自带mContext属性,`InstrumentationTestCase`可以跳转到一个Activity进行测试
1. Android 软键盘盖住输入框或者布局的解决办法
 * 方法一：在你的activity中的oncreate中setContentView之前写上这个代码'getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_ADJUST_PAN);'
 * 方法二：在项目的AndroidManifest.xml文件中界面对应的<activity>里加入`android:windowSoftInputMode="stateVisible|adjustResize"`，这样会让屏幕整体上移。如果加上的是`android:windowSoftInputMode="adjustPan"`这样键盘就会覆盖屏幕。
* TextView的标准字体
```
style="@style/TextAppearance.AppCompat.Display4"
style="@style/TextAppearance.AppCompat.Display3"
style="@style/TextAppearance.AppCompat.Display2"
style="@style/TextAppearance.AppCompat.Display1"
style="@style/TextAppearance.AppCompat.Headline"
style="@style/TextAppearance.AppCompat.Title"
style="@style/TextAppearance.AppCompat.Subhead"
style="@style/TextAppearance.AppCompat.Body2"
style="@style/TextAppearance.AppCompat.Body1"
style="@style/TextAppearance.AppCompat.Caption"
style="@style/TextAppearance.AppCompat.Button"
```
![样式效果](http://upload-images.jianshu.io/upload_images/1181400-62c570276022d2d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* `android:clipToPadding`控件的绘制区域是否在padding里面。默认为true。如果你设置了此属性值为false,可以实现 ListView顶部默认有一个间距，向上滑动后，间距消失。（PS:如果使用margin或padding，都不能实现这个效果。加一个headerView又显得大材小用，而且过于麻烦。此处的clipToPadding配合paddingTop效果就刚刚好。）

* 类似上面`android:clipChildren`的意思：否限制子View在其范围内。在做动画或者一些特别的布局是会起到非常神奇的作用 

* Android5.0以后Theme的一些颜色
![](http://upload-images.jianshu.io/upload_images/1181400-ae85f080aecd4ba4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

to be continue...
