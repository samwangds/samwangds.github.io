>边学习边总结一下安卓自定义View的方法

# 一： 基本工
## 1. 首先继承View
`public class CustomView extends View` 以下是关于View的一些介绍
### 重写`onMeasure(int widthMeasureSpec, int heightMeasureSpec)`方法

* 该方法负责测量大小,如果是View则测量自己,如果是ViewGroup则测量子View和自己。
查看源码可以知道View的`onMeasure`方法通过`setMeasuredDimension(int measuredWidth, int measuredHeight)`方法设置控件大小，以便通过`getMeasuredHeight(),getMeasuredWidth()`获得测量的宽高。在我们重写的时候，也同样需要这样操作。
*  通过参数`MeasureSpec`来帮助获取测量的模式和大小
 * 参数` widthMeasureSpec，heightMeasureSpec `与 android:layout_width 和layout_height的值有关
 * 大小可以通过MeasureSpec.getSize(int measureSpec)获得 （一般情况 下，获得的大小可以直接作为setMeasuredDimension的参数，详细情况要考虑Mode）
 * 模式可以通过MeasureSpec.getMode(int measureSpec)获得 模式分为以下三种
    1. `MeasureSpec.UNSPECIFIED` 未指定模式，父View：老子才不管你，爱咋咋。一般都是父View是AdapterView，通过measure方法传入的模式。
    * `MeasureSpec.EXACTLY` 精确模式，父View：老子说多大就多大，一点都不能偏差，当设置具体值（如：50dp）或者match_parent
    * `MeasureSpec.AT_MOST` 最大模式，子View最大达到指定大小，当设置wrap_content时（这也是为什么要重写的原因，否则在View的默认实现中，wrap_content 就变成了 match_parent 铺满整个父控件 的效果了）
![](http://upload-images.jianshu.io/upload_images/1181400-6954d4b6c14f58a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 * 综上：每个View大小的设定都事由其父View以及该View共同决定的。但这只是一个期望的大小，每个View在测量时最终大小的设定是由setMeasuredDimension()最终决定的。因此，最终确定一个View的“测量长宽“是由以下几个方面影响：
    1. 父View的MeasureSpec属性；
    2. 子View的LayoutParams属性 ；
    3. setMeasuredDimension()或者其它类似设定 mMeasuredWidth 和 mMeasuredHeight 值的方法。


    


>参考资料：
* [自定义View的onMeasure、onLayout](http://yifeiyuan.me/2015/10/12/%E8%87%AA%E5%AE%9A%E4%B9%89View%E7%9A%84onMeasure%E3%80%81onLayout/)
* [实践自定义UI—View](http://www.jianshu.com/p/11210b14f743)
* [[android学习——MeasureSpec介绍及使用](http://www.cnblogs.com/nanxiaojue/p/3536381.html)](http://www.cnblogs.com/nanxiaojue/p/3536381.html?utm_source=tuicool&utm_medium=referral)
