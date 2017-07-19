代码实现 按钮点击出现不同的图片或者颜色



不仅仅是button可以，像TextView也是可以实现的。

## 改变button的颜色 

``` java
		StateListDrawable stateListDrawable = new StateListDrawable();

        ColorDrawable colorDrawableNormal = new ColorDrawable();
        ColorDrawable colorDrawablePressed = new ColorDrawable();

        colorDrawableNormal.setColor(Color.BLUE);
        colorDrawablePressed.setColor(Color.RED);

        //按下状态
        stateListDrawable.addState(new int[]{android.R.attr.state_pressed}, colorDrawablePressed);
 
        //正常状态
        stateListDrawable.addState(new int[]{}, colorDrawableNormal);
        btn1.setBackground(stateListDrawable);
```



##  

## 改变button的背景



       StateListDrawable stateListDrawable = new StateListDrawable();
       
       Drawable drawablePressed =  getResources().getDrawable(R.mipmap.ic_action_add);
    
        //按下状态
        stateListDrawable.addState(new int[]{android.R.attr.state_pressed}, drawablePressed);
     
        //正常状态
        stateListDrawable.addState(new int[]{}, colorDrawableNormal);
        btn1.setBackground(stateListDrawable);