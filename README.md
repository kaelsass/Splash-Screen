OCRus支持三种语言的识别：英语、中文和日文。这三个Tesseract语言包合起来约有70M左右，APK文件中拷贝语言包到手机存储中需要几秒时间，所以我们做了一个启动页面，在为用户展示App第一印象的同时，后台拷贝这三个语言包。经过比较，知乎日报的启动页面有从中心点展开逼进用户的效果，我们决定利用此效果来设计启动页面。最终效果如图所示：
![splashScreen](http://img.blog.csdn.net/20150715144842428)

下面就实现方式进行详细介绍。
## layout.xml
首先创建此页面的布局文件：
``` xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ImageView
        android:id="@+id/imgLogo"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_centerInParent="true"
        android:src="@drawable/splash" />

    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="10dp"
        android:textSize="24dp"
        android:textColor="@color/bright_foreground_material_dark"
        android:gravity="center_horizontal"
        android:layout_alignParentBottom="true"
        android:text="@string/app_name" />

</RelativeLayout>
```
此布局中包含一个ImageView和一个TextView。ImageView即显示的图片，TextView是显示最下面的App名字。

## Activity
然后创建一个新的Activity，在AndroidManifest.xml中将其设置为启动Activity：
``` xml
<activity android:name=".SplashActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```
并在该Activity的onCreate方法中设置图片动画：
``` java
Animation animation = new ScaleAnimation(1.0f, 1.2f, 1.0f, 1.2f, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f); // 将图片放大1.2倍，从中心开始缩放
animation.setDuration(SPLASH_TIME_OUT); // 动画持续时间
animation.setFillAfter(true); // 动画结束后停留在结束的位置
animation.setAnimationListener(this);
splashImage.startAnimation(animation);
```
在动画结束时，画面要跳转到MainActivity。为此，SplashActivity需实现`Animation.AnimationListener`，在`onAnimationEnd`方法中，跳转到MainActivity：
``` java
public void onAnimationEnd(Animation animation) {
    Intent i = new Intent(SplashActivity.this, MainActivity.class);
    startActivity(i);
    finish();
}
```
实现效果如图所示，具体实例详见[Github](https://github.com/kaelsass/Splash-Screen)。
