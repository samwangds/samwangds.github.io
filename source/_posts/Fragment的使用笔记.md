title: Fragment的使用笔记
---

>使用v4包里的Fragment与android.app.Fragment的差别在于：
1. v4包的兼容支持3.0以下版本
2. v4包的Activity必须继承自`android.support.v4.app.FragmentActivity`
* 必须使用`FragmentActivity.getSupportFragmentManager`获取FragmentManager，framework里的Fragment则使用`Activity.getFragmentManager`即可

## 使用方法 
### 1. 最简单的用法，使用Android studio 新建一个Activity 使用Fragment作为内容

![](http://upload-images.jianshu.io/upload_images/1181400-685327ea4308300c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这个时候就会自动生成<fragment>标签，像View一样的使用，其中`android:name`设置为我们的Fragment类所在路径。
```
<fragment android:id="@+id/fragment"
          android:name="pagename.FragmentName"
          android:layout_width="match_parent"
          android:layout_height="match_parent"/>
 ```

###  2.  通过在Activity 实例化Fragment，然后使用`fragmentTransaction.add`添加到`containerView`里
```
        mFragment = new MyFragment();
        FragmentManager fragmentManager = getSupportFragmentManager();
        FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
        fragmentTransaction.add(R.id.containerViewId, mFragment);
        fragmentTransaction.commit();
```
###  3. 多个fragment 动态选择显示在Activity里面
在方法 二的基础上，只要使用以下代码进行Fragment的切换 
replace这个方法，可清除所有已经添加到容器View里面的Fragment，然后再添加我们传入的Fragment。
```
FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
fragmentTransaction.replace(R.id.containerViewId, mFragment2);
fragmentTransaction.commit();
```
###  4. 多个Fragment通过ViewPager显示在Activity里面

 * 首先需要继承 FragmentPagerAdapter 必须实现或重写其中的
 ```
    /**
     * Return the number of views available.   
     */   
   public abstract int getCount();

	/**
     * Return the Fragment associated with a specified position.
     */
    public abstract Fragment getItem(int position);

 ```

* 然后在Activity将Adapter设置到Viewpager里面就可以了
```
FragmentManager fm = getSupportFragmentManager();
mAdapter = new VpAdapter(fm);
mViewPager.setAdapter(mAdapter);
```
* ViewPager和 
    android.support.design.widget.TabLayout更配哦
```
	/**
    *  重写Adapter的这个方法，使TabLayout可以关联显示ViewPager每一页的Title
    */
	public CharSequence getPageTitle(int position);

	//通过以下代码关联TabLayout和ViewPager
	mTabLayout.setupWithViewPager(mViewPager);
    mTabLayout.setTabsFromPagerAdapter(mAdapter);
```

## Fragment的简单介绍
### 生命周期
![Fragment必须是依存与Activity而存在的，官网这张图很好的说明了两者生命周期的关系](http://upload-images.jianshu.io/upload_images/1181400-063dc0b2dc9f900d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到Fragment比Activity多了几个额外的生命周期回调方法：
`onAttach(Activity)` 当Fragment与Activity发生关联时调用。
`onCreateView(LayoutInflater, ViewGroup,Bundle)`创建该Fragment的视图
`onActivityCreated(Bundle)`当Activity的onCreate方法返回时调用
`onDestoryView()`与onCreateView相对应，当该Fragment的视图被移除时调用
`onDetach()`与onAttach相对应，当Fragment与Activity关联被取消时调用
注意：除了onCreateView，其他的所有方法如果你重写了，必须调用父类对于该方法的实现

### Fragment 常用的API
Fragment常用的三个类：
Fragment 主要用于定义Fragment
FragmentManager 主要用于在Activity中操作Fragment
FragmentTransaction 保证一些列Fragment操作的原子性 
**主要的操作都是FragmentTransaction的方法**
FragmentTransaction transaction = fm.benginTransatcion();//开启一个事务
**transaction.add() **往Activity中添加一个Fragment
**transaction.remove() **从Activity中移除一个Fragment，如果被移除的Fragment没有添加到回退栈（回退栈后面会详细说），这个Fragment实例将会被销毁。
**transaction.replace()**使用另一个Fragment替换当前的，实际上就是remove()然后add()的合体~
**transaction.hide()**隐藏当前的Fragment，仅仅是设为不可见，并不会销毁
**transaction.show()**显示之前隐藏的Fragment
**transaction.detach()**会将view从UI中移除,和remove()不同,此时fragment的状态依然由FragmentManager维护。
**transaction.attach()**重建view视图，附加到UI上并显示。
**transatcion.commit()**提交一个事务
### Fragment栈管理
通过`FragmentTransaction.addToBackStack()`可以将事务添加进回退栈中(`back stack`)，这样在事务提交之后，可以从栈中再将这个事务中取出（即退按后退键可以回到原来的状态）
```
  FragmentManager fragmentManager = getSupportFragmentManager();
  FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
  mFragment2 = new MainActivityFragment2();
  fragmentTransaction.replace(R.id.fragment, mFragment2);
  fragmentTransaction.addToBackStack(null);
  fragmentTransaction.commit();
```
注意Fragment被remove之后View会被销毁，不管是否有添加到栈中，所以比如在Fragment中有输入一些数据，销毁后再回退回去，双重新createView则原来Fragment中的输入的数据将会丢失，这种场景下，可以使用**`transaction.hide()`**然后再**`transaction.add()` **新的Fragment，这样才回退的时候，才能完整恢复原来的Fragment。
### Fragment与Activity通信
因为Fragment是依附于Activity的，所以通信起来并不复杂
1. 使用Fragment的引用（通过实例化new 出来,或者通过`FragmentManager.findFragmentByTag() 和 findFragmentById()`获取），可以通过引用直接访问所有的Fragment的public方法
2. Fragment中可以通过getActivity()得到当前绑定的Activity的实例，然后进行操作访问Activity的所有public方法。
3. Fragment需要Context，可以通过调用getActivity(),如果该Context需要在Activity被销毁后还存在，则使用getActivity().getApplicationContext()
* 广播，EventBus等方法进行通信



