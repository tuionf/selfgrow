---
title: Glide的使用 
tags: Android,小书匠
grammar_cjkRuby: true
---

# 简单使用 

1. 引入 

``` java
dependencies {  
    compile 'com.github.bumptech.glide:glide:3.5.2'  
    compile 'com.android.support:support-v4:22.0.0'  
}
```

2. 使用 

``` java
Glide.with(MainActivity.this)
                .load(url)
                .into(imageView);
```

2.1 Glide的with()  

可以接收的参数：

 - Context context;
 - Activity activity;
 - FragmentActivity fragmentActivity;
 - Fragment fragment;

2.2 load()是加载目标资源

可以接收的参数类型：

 - Uri uri;
 -  String uriString;
   -  File file;
    - Integer resourceId;
    - byte[] model;
    - String model;


2.3 into()就是加载资源完成后作什么处理，三种方式处理 

1）into(ImageView imageView);//显示在控件上

2）into(Target target);//通过回调获得加载结果

``` java
Glide.with(MainActivity.this)
                .load("https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1488304980120&di=691744515390177625e5708270f84654&imgtype=0&src=http%3A%2F%2Fpic.58pic.com%2F58pic%2F17%2F16%2F41%2F06G58PICfVz_1024.jpg")
                .into(new SimpleTarget<GlideDrawable>() {
                    @Override
                    public void onResourceReady(GlideDrawable resource, GlideAnimation<? super GlideDrawable> glideAnimation) {
                        
                    }
                });
```
3）into(int w, int h);//指定期望的图片大小，返回一个future对象，要在子线程中获取

``` stylus
new Thread(new Runnable() {
        @Override
        public void run() {
            Bitmap bitmap = Glide.with(context)  
                .load("xxxx.png")  
                .into(200， 200)
                .get();
        }
    }).start();
```

2.4 图片尺寸

``` java
Glide.with(this).load(mUrl).override(300,300).into(mIv);
```
## 区别 

第一种是直接把图片加载出来显示在ImageView上，你并不会得到中间结果，也就是bitmap对象。 
第二种、第三种都是可以获取到bitmap的，获取到后怎么处理自己决定。 
第三种需要在子线程中调用

## 占位图以及加载动画

1.设置占位图(placeholder) 

有时候加载的图片过大时,或者网络不好时,我们经常希望控件在加载过程中有一张默认的占位图

``` java
Glide.with(this).load(url).placeholder(R.mipmap.place).into(iv);
```

2. 设置错误图片 

``` java
Glide.with(this).load(url).placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).into(iv);
```

3. 设置动画(crossFade)

Glide默认是包含淡入淡出动画的时间为300ms(毫秒),我们可以修改这个动画的时间

``` java
Glide.with(this).load(url).placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).crossFade(5000).into(iv);

```
4. 取消动画(dontAnimate)
当我们不希望有淡入淡出动画时

``` java
Glide.with(this).load(url).placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).dontAnimate().into(iv);
```

## 加载本地图片 

1. 添加权限 

``` xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

2. 代码 

``` java
    //本地文件
    File file = new File(Environment.getExternalStorageDirectory(), "xiayu.png");
    //加载图片
    Glide.with(this).load(file).into(iv);
```

## 加载Gif

1.简单加载Gif

``` java
Glide.with(this).load(mGifUrl).placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).into(mIv);
```

2.把gif当作普通图片加载(asBitmap)

只加载gif的第一帧,把gif当作普通图片一样加载,那么只需要加上asBitmap方法即可

``` java
Glide.with(this).load(mGifUrl).asBitmap().placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).into(mIv);
```
3. 检查是否gif(asGif)

如果你希望加载的只是gif,如果不是gif就显示错误图片,那么只用加上asGif方法即可

``` java
 Glide.with(this).load(mGifUrl).asGif().placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).into(mIv);
```

4.加载本地视频缩略图

Glide只会加载本地视频的第一帧,也就是缩略图,而且其实加载缩略图的时候也无需转化为Uri,直接把File丢进去就行了

``` java
    mVideoFile = new File(Environment.getExternalStorageDirectory(), "xiayu.mp4");

Glide.with(this).load(mVideoFile).placeholder(R.mipmap.place).error(R.mipmap.icon_photo_error).into(mIv);
```
5.其他

在大多数情况下，当你使用diskCacheStrategy（DiskCacheStrategy.SOURCE）时，Gif的加载速度会显着提高(其实就是把gif资源缓存到磁盘)

Glide.with(this).load(mGifUrl).diskCacheStrategy(DiskCacheStrategy.SOURCE).placeholder(R.mipmap.place).error(R.mipmap.icon_photo_erro

## 绑定生命周期

其实Glide与activity和fragment绑定生命周期很简单,只用在with的时候传入想绑定生命周期的Context就行.

``` java
Glide.with(this).load(mUrl).into(mIv);
```
## 内存缓存和磁盘缓存 

1.缓存的资源

Glide的缓存资源分为两种:

- .原图(SOURCE) :原始图片
- 处理图(RESULT) :经过压缩和变形等处理后的图片

2.内存缓存策略(skipMemoryCache)

Glide默认是会在内存中缓存处理图(RESULT)的.

可以通过调用skipMemoryCache(true)来设置跳过内存缓存

    //跳过内存缓存
    Glide.with(this).load(mUrl).skipMemoryCache(true).into(mIv);

3.磁盘缓存策略(diskCacheStrategy)

Glide磁盘缓存策略分为四种,默认的是RESULT——默认值

- ALL:缓存原图(SOURCE)和处理图(RESULT)

- NONE:什么都不缓存

- SOURCE:只缓存原图(SOURCE)

-  RESULT:只缓存处理图(RESULT) —默认值

4.组合策略

和其他三级缓存一样,Glide的缓存读取顺序是 **内存–>磁盘–>网络**

需要注意的是Glide的内存缓存和磁盘缓存的配置相互没有直接影响,所以可以同时进行配置

1.内存不缓存,磁盘缓存缓存所有图片

``` lasso
Glide.with(this).load(mUrl).skipMemoryCache(true).diskCacheStrategy(DiskCacheStrategy.ALL).into(mIv);
```


2.内存缓存处理图,磁盘缓存原图

``` gradle
Glide.with(this).load(mUrl).skipMemoryCache(false).diskCacheStrategy(DiskCacheStrategy.SOURCE).into(mIv);
```
5.缓存大小及路径

5.1 内存缓存最大空间

Glide的内存缓存其实涉及到比较多的计算,这里就介绍最重要的一个参数,就是**内存缓存最大空间**

内存缓存最大空间(maxSize) = 每个进程可用的最大内存  *  0.4 

(低配手机的话是: 每个进程可用的最大内存 * 0.33)

5.2 磁盘缓存大小

磁盘缓存大小: 250 * 1024 * 1024(250MB)


5.3 磁盘缓存目录

磁盘缓存目录: 项目/cache/image_manager_disk_cache


6 . 清除缓存

6.1 清除所有缓存

清除所有**内存**缓存(**需要在Ui线程操作**)

``` java
Glide.get(this).clearMemory();
```


清除所有**磁盘**缓存(需要在**子线程**操作)

``` java
Glide.get(MainActivity.this).clearDiskCache();
```

注:**在使用中的资源不会被清除**

6.2 清除单个缓存

由于Glide可能会缓存一张图片的多个分辨率的图片,并且文件名是被哈希过的,所以并不能很好的删除单个资源的缓存



## 图片预处理 

比如说圆角处理,圆形图片,高斯模糊等

1. 创建一个类继承BitmapTransformation，实现transform方法 

``` java
public  class CornersTransform extends BitmapTransformation {

        @Override
        protected Bitmap transform(BitmapPool pool, Bitmap toTransform, int outWidth, int outHeight) {
            //TODO  在这里对图片作处理
        }



        @Override
        public String getId() {
           //TODO
        }
}
```
2. 使用

通过调用transform方法就能展示处理后的图片

``` java
Glide.with(this).load(url).transform(new CornersTransform()).into(iv1);
```

举例：http://blog.csdn.net/yulyu/article/details/55261351