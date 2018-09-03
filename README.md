# Glide

  Glide，一个被  google  所推荐的图片加载库，作者是 bumptech 。这个库被广泛运用在 google 的开源项目中。经过多年的迭代，Glide 已经成为了安卓开发者最喜爱的图片加载库之一。新版本的使用方式和以前的 3.x.x 在使用存在区别，以下是演示最新版本的 Glide 的使用，Glide 新的版本也做了较多的优化和更多功能的实现。
  

## 一、功能介绍

  专注于图片的下载处理


## 二、使用

### 配置到 Android 项目

Gradle 方式

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
### 1.  代码简单使用

 

```
Glide.with(this)
     .load("https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1524215585000&di=bfce834d92cbdd3ded74d695cf5c8638&imgtype=0&src=http%3A%2F%2Fimage5.tuku.cn%2Fpic%2Fwallpaper%2Fmeinv%2Frenbihuajiaominv%2F010.jpg")
     .into(ivImage);

```

取消加载图片

```
Glide.with(fragment).clear(imageView);

```

###  2.  加载占位图

顾名思义，占位图就是指在图片的加载过程中，我们先显示一张临时的图片，等图片加载出来了再替换成要加载的图片。

下面我们就来学习一下  Glide 占位图功能的使用方法，首先我事先准备好了一张  loading.jpg  图片，用来作为占位图显示。然后修改  Glide  加载部分的代码，如下所示：

加载过程中的占用  (Placeholder)
 
 ```
RequestOptions options = new RequestOptions()
        .placeholder(R.drawable.loading);
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);

 
 ```
 
 没错，就是这么简单。这里我们先创建了一个  RequestOptions 对象，然后调用它的  placeholder()  方法来指定占位图，再将占位图片的资源  id  传入到这个方法中。最后，在 Glide 的三步走之间加入一个  apply()  方法，来应用我们刚才创建的  RequestOptions  对象。
 
 不过如果你现在重新运行一下代码并点击 Load Image，很可能是根本看不到占位图效果的。因为  Glide  有非常强大的缓存机制，我们刚才加载图片的时候  Glide  自动就已经将它缓存下来了，下次加载的时候将会直接从缓存中读取，不会再去网络下载了，因而加载的速度非常快，所以占位图可能根本来不及显示。
 
 因此这里我们还需要稍微做一点修改，来让占位图能有机会显示出来，修改代码如下所示：

  ```
RequestOptions options = new RequestOptions()
        .placeholder(R.drawable.loading)
        .diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);


 ```
 
 可以看到，这里在   RequestOptions  对象中又串接了一个 diskCacheStrategy() 方法，并传入 DiskCacheStrategy.NONE  参数，这样就可以禁用掉  Glide  的缓存功能。
 

除了这种加载占位图之外，还有一种异常占位图。异常占位图就是指，如果因为某些异常情况导致图片加载失败，比如说手机网络信号不好，这个时候就显示这张异常占位图。

异常占位图的用法相信你已经可以猜到了，首先准备一张  error.jpg  图片，然后修改  Glide  加载部分的代码，如下所示：
 
   ```
RequestOptions options = new RequestOptions()
        .placeholder(R.drawable.ic_launcher_background)
        .error(R.drawable.error)
        .diskCacheStrategy(DiskCacheStrategy.NONE);
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);

	   
```
很简单，这里又串接了一个 error() 方法就可以指定异常占位图了。

其实看到这里，如果你熟悉  Glide 3  的话，相信你已经掌握  Glide 4  的变化规律了。在  Glide 3  当中，像placeholder()、error()、diskCacheStrategy()  等等一系列的  API，都是直接串联在  Glide  三步走方法中使用的。

而  Glide 4  中引入了一个  RequestOptions  对象，将这一系列的  API  都移动到了  RequestOptions  当中。这样做的好处是可以使我们摆脱冗长的  Glide  加载语句，而且还能进行自己的  API  封装，因为  RequestOptions是可以作为参数传入到方法中。



### 3. 指定图片大小

实际上，使用  Glide 在大多数情况下我们都是不需要指定图片大小的，因为 Glide 会自动根据  ImageView 的大小来决定图片的大小，以此保证图片不会占用过多的内存从而引发 OOM。

不过，如果你真的有这样的需求，必须给图片指定一个固定的大小，Glide  仍然是支持这个功能的。修改  Glide加载部分的代码，如下所示：

 ```
RequestOptions options = new RequestOptions()
        .override(200, 100);
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);
     
```
仍然非常简单，这里使用  override()  方法指定了一个图片的尺寸。也就是说，Glide  现在只会将图片加载成200*100  像素的尺寸，而不会管你的ImageView的大小是多少了。

如果你想加载一张图片的原始尺寸的话，可以使用  Target.SIZE_ORIGINAL  关键字，如下所示：

 ```
RequestOptions options = new RequestOptions()
        .override(Target.SIZE_ORIGINAL);
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);

 ```
 
 ###  4.  指定加载格式
 
 我们都知道，Glide  其中一个非常亮眼的功能就是可以加载  GIF  图片，而同样作为非常出色的图片加载框架的Picasso  是不支持这个功能的。

而且使用   Glide  加载  GIF  图并不需要编写什么额外的代码，Glide  内部会自动判断图片格式。比如我们将加载图片的  URL  地址改成一张GIF图，如下所示：

 ```
  Glide.with(this)
     .load("http://guolin.tech/test.gif")
     .into(imageView);

 ```
 
 ### 5.  图片变换
 
 图片变换的意思就是说，Glide  从加载了原始图片到最终展示给用户之前，又进行了一些变换处理，从而能够实现一些更加丰富的图片效果，如图片圆角化、圆形化、模糊化等等。

添加图片变换的用法非常简单，我们只需要在  RequestOptions  中串接  transforms()  方法，并将想要执行的图片变换操作作为参数传入  transforms()  方法即可，如下所示：

```
RequestOptions options = new RequestOptions()
        .transforms(...);
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);

```
至于具体要进行什么样的图片变换操作，这个通常都是需要我们自己来写的。不过  Glide  已经内置了几种图片变换操作，我们可以直接拿来使用，比如  CenterCrop、FitCenter、CircleCrop  等。

但所有的内置图片变换操作其实都不需要使用  transform()  方法，Glide  为了方便我们使用直接提供了现成的API：

```
RequestOptions options = new RequestOptions()
        .centerCrop();
 
RequestOptions options = new RequestOptions()
        .fitCenter();
 
RequestOptions options = new RequestOptions()
        .circleCrop();


```

当然，这些内置的图片变换  API  其实也只是对  transform()  方法进行了一层封装而已，它们背后的源码仍然还是借助  transform()  方法来实现的。

这里我们就选择其中一种内置的图片变换操作来演示一下吧，circleCrop()  方法是用来对图片进行圆形化裁剪的，我们动手试一下，代码如下所示：

```
String url = "http://guolin.tech/book.png";
RequestOptions options = new RequestOptions()
        .circleCrop();
Glide.with(this)
     .load(url)
     .apply(options)
     .into(imageView);


```
