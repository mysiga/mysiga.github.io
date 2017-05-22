title: Android单元测试框架
date: 2016-03-18  10:30:00
tags:
categories: 单元测试

---
## Android单元测试框架
- Android单元测试
	- 在Android项目中，单元测试的对象是组件状态、控件行为、界面元素和自定义函数。不推荐对每个函数进行一对一的测试，像onStart()、onDestroy()这些周期函数并不需要全部覆盖到。  
- 框架比较
	- JUnit
		- 原生单元测试
		- 不能让Activity执行到resume的状态
		- @王进已讲
	- AndroidTest
		- 运行在Android环境
	- Instrumentation
		- 运行在模拟器上
	- Robotium
		- 类似于Selenium的测试框架
		- 运行在模拟器上
		- 通过对模拟器的操作或者mock，来触发函数调用，进而对其结果进行验证
	- Robolectric+Mock(Mockito)
		- 运行在JVM上速度快
		- Jenkins周期性执行
		- 无需准备Android环境
		- 实现了Android中对XML的解析，模拟了View，Layout，以及资源的加载
		- 一些测试对象依赖度较高而需要解除依赖的场景(如网络)使用Mock框架
- [Robolectric](http://robolectric.org/getting-started/)
	- Android Studio配置
		-  build.gradle添加库
		
		````
		testCompile "org.robolectric:robolectric:3.0"
		````
		- Build Variants设置
		![Unit Tests](http://robolectric.org/images/android-studio-enable-unit-tests-f15bd816.png)
		- Linux和Mac平台的需要配置下才能运行:在Android Studio中run边上有一个app选择，这里选择"Edit Configurations" 出现编辑界面
		![Run/Debug Configurations]()
		在左边中新建JUnit，Default->JUnit
		- 测试代码
		- 中间出现问题
			- Android SDK目前只支持到22，5.1
- 单元测试的目标函数主要有三种：
	1. 有明确的返回值，如上图的dosomething(Boolean param)，做单元测试时，只需调用这个函数，然后验证函数的返回值是否符合预期结果。
	2. 这个函数只改变其对象内部的一些属性或者状态，函数本身没有返回值，就验证它所改变的属性和状态。
	3. 	一些函数没有返回值，也没有直接改变哪个值的状态，这就需要验证其行为，比如点击事件。 
	
	- 既没有返回值，也没有改变状态，又没有触发行为的函数是不可测试的，在项目中不应该存在。当存在同时具备上述多种特性时，本文建议采用多个case来真对每一种特性逐一验证，或者采用一个case，逐一执行目标函数并验证其影响。
	- 构造用例的原则是测试用例与函数一对一，实现条件覆盖与路径覆盖。Java单元测试中，良好的单元测试是需要保证所有函数执行正确的，即所有边界条件都验证过，一个用例只测一个函数，便于维护。在Android单元测试中，并不要求对所有函数都覆盖到，像Android SDK中的函数回调则不用测试。
- 测试用例
	- 组件
		- Activity测试
		
		````
		@Test
    public void testLifecycle() {
        ActivityController<MainActivity> activityController = Robolectric.buildActivity(MainActivity.class).create().start();
        Activity activity = activityController.get();
        Button action_next = (Button) activity.findViewById(R.id.next);
        Assert.assertEquals("next", action_next.getText().toString());
//        error
//        Assert.assertEquals("testLifecycle", action_next.getText().toString());
    }

		````  
		- Service
		- BroadcastReceiver
		- Fragment 
	- 控件行为
		- 跳转
	- 界面元素
		- Toast
		
		````
		@Test
    public void testToast() {
        Button action_toast = (Button) mActivity.findViewById(R.id.action_toast);
        action_toast.performClick();
        Assert.assertEquals("hello", ShadowToast.getTextOfLatestToast());
    }
		````
		- Dialog
		- UI控件
		
		````
		 @Test
    public void testStartEndActivity() {
        Button action_next = (Button) mActivity.findViewById(R.id.next);
        action_next.performClick();

        Intent intent = new Intent(mActivity, EndActivity.class);

        Intent actionIntent = ShadowApplication.getInstance().getNextStartedActivity();
        Assert.assertEquals(intent, actionIntent);
    }
		````
	- 自定义函数
	- 网络请求
- assertJ-android断言库
- 资料
	- [参考1](https://hkliya.gitbooks.io/unit-test-android-with-robolectric/content/0-introduction.html)
	- [美团Android单元测试](http://tech.meituan.com/Android_unit_test.html)
	- [测试用例](http://www.jianshu.com/p/9d988a2f8ff7)
	