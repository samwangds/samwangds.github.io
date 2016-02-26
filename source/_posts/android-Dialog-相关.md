# 宽度铺满
在`setContentView`之后调用以下代码
```
WindowManager.LayoutParams lp = getWindow().getAttributes();
lp.width = ViewGroup.LayoutParams.MATCH_PARENT;
getWindow().getDecorView().setPadding(0,0,0,0);
getWindow().setAttributes(lp);
```
