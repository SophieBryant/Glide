# Glide

   Glide，一个被google所推荐的图片加载库，作者是bumptech。这个库被广泛运用在google的开源项目中。经过多年的迭代，Glide已经成为了安卓开发者最喜爱的图片加载库之一。新版本的使用方式和以前的3.x.x在使用存在区别，以下是演示最新版本的Glide的使用，Glide新的版本也做了较多的优化和更多功能的实现。
  

## 一、功能介绍

  专注于图片的下载处理


## 二、使用

### 配置到Android项目

Gradle方式

```
repositories {
  mavenCentral()
  google()
}

dependencies {
  implementation 'com.github.bumptech.glide:glide:4.7.1'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.7.1'
}

```
添加混肴

```
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public class * extends com.bumptech.glide.module.AppGlideModule
-keep public enum com.bumptech.glide.load.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}
-dontwarn com.bumptech.glide.load.resource.bitmap.VideoDecoder
# for DexGuard only
-keepresourcexmlelements manifest/application/meta-data@value=GlideModule

```
添加权限
```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

```
###  代码简单使用

 

```
Glide.with(this)
     .load("https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1524215585000&di=bfce834d92cbdd3ded74d695cf5c8638&imgtype=0&src=http%3A%2F%2Fimage5.tuku.cn%2Fpic%2Fwallpaper%2Fmeinv%2Frenbihuajiaominv%2F010.jpg")
     .into(ivImage);

```
