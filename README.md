

DiscreteSeekbar在github上的项目主页是：https://github.com/AnderWeb/discreteSeekBar

DiscreteSeekbar可以自定制的属性很多，可以在其github的项目主页上查看。DiscreteSeekbar可以像Android 原生的Seekbar一样使用。

使用方法：

写布局activity_main.xml：


复制代码
 1 <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
 2     xmlns:tools="http://schemas.android.com/tools"
 3     xmlns:app="http://schemas.android.com/apk/res-auto"
 4     android:layout_width="match_parent"
 5     android:layout_height="match_parent"
 6     android:orientation="vertical"
 7     tools:context="com.zzw.testdiscreteseekbar.MainActivity" >
 8 
 9     <!-- app:dsb_max 最大值 -->
10     <!-- app:dsb_min 最小值 -->
11     <!-- app:dsb_value 当前值 -->
12 
13     <org.adw.library.widgets.discreteseekbar.DiscreteSeekBar
14         android:id="@+id/discreteSeekBar1"
15         android:layout_width="match_parent"
16         android:layout_height="wrap_content"
17         android:layout_marginTop="50dp"
18         app:dsb_max="10"
19         app:dsb_min="-10"
20         app:dsb_value="0" />
21 
22     <!-- app:dsb_indicatorFormatter="值 %d"表示显示出来的值形如：值 37 -->
23     <!-- app:dsb_indicatorColor="@android:color/holo_red_light" 气泡显示的颜色 -->
24     <!-- app:dsb_rippleColor="@android:color/holo_red_light" 手指拉动时候手指位置的颜色 -->
25 
26     <org.adw.library.widgets.discreteseekbar.DiscreteSeekBar
27         android:id="@+id/discreteSeekBar2"
28         android:layout_width="match_parent"
29         android:layout_height="wrap_content"
30         android:layout_marginTop="50dip"
31         app:dsb_allowTrackClickToDrag="false"
32         app:dsb_indicatorColor="@android:color/holo_red_light"
33         app:dsb_indicatorFormatter="值 %d"
34         app:dsb_rippleColor="@android:color/holo_red_light" />
35 
36     <!-- app:dsb_indicatorFormatter="%04d"代表有几位数 ，0后面是几就是几位数  如：0013，0135，1000 -->
37 
38     <org.adw.library.widgets.discreteseekbar.DiscreteSeekBar
39         android:id="@+id/discreteSeekBar3"
40         android:layout_width="match_parent"
41         android:layout_height="wrap_content"
42         android:layout_marginTop="50dip"
43         app:dsb_indicatorColor="@android:color/holo_green_light"
44         app:dsb_indicatorFormatter="%04d"
45         app:dsb_max="1000"
46         app:dsb_min="1"
47         app:dsb_rippleColor="@android:color/holo_blue_light" />
48 
49 </LinearLayout>

复制代码

MainActivity.java可以设置参数：


复制代码
 1 package com.zzw.testdiscreteseekbar;
 2 
 3 import org.adw.library.widgets.discreteseekbar.DiscreteSeekBar;
 4 import org.adw.library.widgets.discreteseekbar.DiscreteSeekBar.NumericTransformer;
 5 import org.adw.library.widgets.discreteseekbar.DiscreteSeekBar.OnProgressChangeListener;
 6 
 7 import android.app.Activity;
 8 import android.os.Bundle;
 9 import android.util.Log;
10 
11 public class MainActivity extends Activity {
12 
13     protected static final String TAG = "MainActivity";
14 
15     @Override
16     protected void onCreate(Bundle savedInstanceState) {
17         super.onCreate(savedInstanceState);
18         setContentView(R.layout.activity_main);
19 
20         DiscreteSeekBar discreteSeekBar1 = (DiscreteSeekBar) findViewById(R.id.discreteSeekBar1);
21         discreteSeekBar1.setNumericTransformer(new NumericTransformer() {
22 
23             @Override
24             public int transform(int value) {
25 
26                 return value * 100;
27             }
28         });
29 
30         
31         DiscreteSeekBar discreteSeekBar2 = (DiscreteSeekBar) findViewById(R.id.discreteSeekBar2);
32         discreteSeekBar2.setOnProgressChangeListener(new OnProgressChangeListener() {
33             
34             @Override
35             public void onStopTrackingTouch(DiscreteSeekBar seekBar) {
36                 
37             }
38             
39             @Override
40             public void onStartTrackingTouch(DiscreteSeekBar seekBar) {
41                 
42             }
43             
44             @Override
45             public void onProgressChanged(DiscreteSeekBar seekBar, int value,
46                     boolean fromUser) {
47                 Log.d(TAG, value+"");
48             }
49         });
50     }
51 
52 }

复制代码

 


#DiscreteSeekBar

![screenshot](https://lh6.googleusercontent.com/-JjvxVMCm1ug/VHUPWVBfpbI/AAAAAAAAHtQ/TPtoOjHI5MA/w639-h356/seekbar2.gif)

![screenshot](https://lh3.googleusercontent.com/-7nbVPXxUhYk/VG-rO64pMWI/AAAAAAAAHsM/aMRglt2Vzrk/w639-h480/animation.gif)

DiscreteSeekbar is my poor attempt to develop an android implementation of the [Discrete Slider] component from the Google Material Design Guidelines.

##Prologe
I really hope Google provides developers with a better (and official) implementation ;)

##Warning
After a bunch of hours trying to replicate the exact feel of the Material's Discrete Seekbar, with a beautiful stuff-that-morphs-into-other-stuff animation I was convinced about releasing the current code.

```java
android.util.Log.wtf("WARNING!! HACKERY-DRAGONS!!");
```
I've done a few bit of hacky cede and a bunch of things I'm not completely proud of, so use under your sole responsibility (or help me improve it via pull-requests!)

##Implementation details
This thing runs on minSDK=7 (well, technically could run 4 but can't test since AVDs for api 4 are deprecated and just don't boot).
Obviously some of the subtle animations (navigating with the Keyboard, the Ripple effect, text fade ins/fade outs, etc) are not going to work on APIS lower than 11, but the bubble thing does. And I haven't found a way of improving this with 11-21 APIs, so...

The base SeekBar is pretty simple. Just 3 drawables for the track, progress and thumb. Some touch event logic to drag, some key event logic to move, and that's all.

It supports custom ranges (custom min/max), even for negative values.

The bubble thing **DOESN'T USE** [VectorDrawableMagic] . I was not really needed for such a simple morph. It uses instead an [Animatable Drawable] for the animation with a lot of hackery for callbacks, drawing and a bunch of old simple math.

>For this to work (and sync with events, etc) I've written a fair amount of shit questionable code...

The material-floating-thing is composed into the WindowManager (like the typical overflow menus) to be able to show it over other Views without needing to set the SeekBar big enough to account for the (variable) size of he floating thing.

>For this I'm not sure about the amounts of things I've copied from [PopupWindow] and the possible issues.

##Dependencies
It uses **com.android.support:support-v4** as the only dependency.

##Usage
This is published in jCenter so you need to use the appropiate repo:

```groovy
repositories {
    jcenter()
}

dependencies {
    compile 'org.adw.library:discrete-seekbar:1.0.0'
}
```

Once imported into your project, you just need to put them into your layous like:

```xml
<org.adw.library.widgets.discreteseekbar.DiscreteSeekBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:dsb_min="2"
        app:dsb_max="15"
/>
```

####Parameters
You can tweak a few things of the DiscreteSeekbar:

* **dsb_min**: minimum value
* **dsb_max**: maximum value
* **dsb_value**: current value
* **dsb_mirrorForRtl**: reverse the DiscreteSeekBar for RTL locales
* **dsb_allowTrackClickToDrag**: allows clicking outside the thumb circle to initiate drag. Default TRUE
* **dsb_indicatorFormatter**: a string [Format] to apply to the value inside the bubble indicator.

####Design
 
* **dsb_progressColor**: color/colorStateList for the progress bar and thumb drawable
* **dsb_trackColor**: color/colorStateList for the track drawable
* **dsb_indicatorTextAppearance**: TextAppearance for the bubble indicator
* **dsb_indicatorColor**: color/colorStateList for the bubble shaped drawable
* **dsb_indicatorElevation**: related to android:elevation. Will only be used on API level 21+
* **dsb_rippleColor**: color/colorStateList for the ripple drawable seen when pressing the thumb. (Yes, it does a kind of "ripple" on API levels lower than 21 and a real RippleDrawable for 21+.

You can also use the attribute **discreteSeekBarStyle** on your themes with a custom Style to be applied to all the DiscreteSeekBars on your app/activity/fragment/whatever.

##License
```
Copyright 2014 Gustavo Claramunt (Ander Webbs)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[Discrete Slider]:http://www.google.com/design/spec/components/sliders.html#sliders-discrete-slider
[VectorDrawableMagic]:https://developer.android.com/reference/android/graphics/drawable/AnimatedVectorDrawable.html
[Animatable Drawable]:https://developer.android.com/reference/android/graphics/drawable/Animatable.html
[PopupWindow]:https://developer.android.com/reference/android/widget/PopupWindow.html
[Format]:https://developer.android.com/reference/java/util/Formatter.html

