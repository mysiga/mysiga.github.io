title: Android动画
date: 2021-08-30  19:20:00
tags:
categories: Android

---



#### Android动画分类
 - Frame Animation(帧动画)：
    - 使用AnimationDrawable配置XML，按顺序播放事先做好的图像，
    - 使用AnimatedVectorDrawableCompat在xml中定义一组动画
 - Tween Animation(补间动画):
只能对View进行平移，缩放，渐变，旋转
 - Property Animation(属性动画):
API11开始引入，基于Object进行属性改变而达到动画效果

 #### 使用动画场景
   - [View动画](https://developer.android.google.cn/training/animation/overview)
   - [为布局添加动画：适用于Activity或Fragment内切换布局时的动画](https://developer.android.google.cn/training/transitions)
    - [Activiy转场动画](https://developer.android.com/training/transitions/start-activity)
    - [为布局更改添加动画](https://developer.android.com/training/transitions)

#### 动画相关API类
- [MotionLayout](https://developer.android.com/training/constraint-layout/motionlayout?hl=zh-cn)
- [ValueAnimator](https://developer.android.google.cn/reference/android/animation/ValueAnimator)
- [AnimatedVectorDrawable](https://developer.android.google.cn/guide/topics/graphics/drawable-animation#AnimVector)
- [AnimationDrawable](https://developer.android.google.cn/guide/topics/graphics/drawable-animation#AnimationDrawable)
- [FragmentTransaction](https://developer.android.google.cn/reference/android/app/FragmentTransaction)
- [ObjectAnimator](https://developer.android.google.cn/reference/android/animation/ObjectAnimator)
- [Transition](https://developer.android.google.cn/reference/android/transition/Transition)
- [ActivityOptions](https://developer.android.com/reference/android/app/ActivityOptions)


#### 第三方动画库
- [SVGAPlayer-Android](https://github.com/svga/SVGAPlayer-Android)
- [lottie](https://github.com/airbnb/lottie-android)