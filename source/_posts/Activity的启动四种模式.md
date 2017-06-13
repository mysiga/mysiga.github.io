title: Activity的启动四种模式
date: 2016-03-05 17:42:29
tags:
categories: Android
---


#### Activity的启动四种模式
- standard(默认)
	- 每次使用startActivity方法启动Activity时
   都会创建该Activity的新实例
 	- Activity的多个实例可以位于同一个task中
   也可以分布在不同的task里
 	- 在当前task中使用startActivity方法启动Activity
   也在当前task中创建和运行
- singleTop：栈顶复用模式
 	- 当设置为singleTop模式Activity，未处于栈顶时
   其运行特点与标准模式下一致
 	- 当其位于栈顶时，如果使用startActivity方法
   再次启动该Activity，则不会创建该Activity的新实例
   而是直接使用栈顶的实例响应启动操作.并且调用该实例
   的onNewIntent方法，将本次启动的intent传入到实例中
- singleTask：栈内复用模式
 	- 设置singleTask的Activity，具有全局唯一性
   即同一时刻在Android系统中只能存在该Activity
   的一个实例
 	- 当使用startActivity方法启动设置为singleTask的Activity时
  如果该Activity的实例尚不存在，则创建该实例
  否则，不会重复创建实例，而是将已经存在的实例重新带回到栈顶
  如果在该实例上存在其他实例，则销毁这些实例后，将该实例重新
  带回栈顶
  - 在创建该Activity的实例时，
  如果当前任务的taskAffinity值与该Activity的taskAffinity值相同，
   则在当前任务中创建实例，
  否则，在一个新的任务中创建实例
  - 具有clearTop的效果，如果再次启动具有实例的singleTask的Acitivity会清除它上面的Activity出栈

- singleInstance
 	- 设置singleInstance的Activity，具有全局唯一性
   即同一时刻在Android系统中只能存在该Activity
   的一个实例
 	- 当使用startActivity方法启动该Activity的实例时
   如果该Activity的实例尚不存在，则直接在新的任务
   中创建该实例。
   如果该实例已经存在，则直接显示该实例
 	- 该Activity的实例，不与其他activity的实例共存于一个
   任务中,所以通过该实例启动的其他Activity的实例，只能
   被创建在其他的task里，如果已经存在于新创建的activity实例
   taskAffinity值相同的task，则直接在该task中创建实例，否则
   就在一个新的任务中创建实例

- Acitivity启动模式优先级
 	- 第一种
 	
 ````
 <activity
            android:name=".launchmode.SingleTopActivity"
            android:label="@string/title_activity_single_top"
            android:launchMode="singleTop"
            android:theme="@style/AppTheme" />
 ````
 	- 第二种
 	
 	````
 	Intent intent=new Intent(this,SingleTopActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
                startActivity(intent);
 	````
 	第二种优先第一种，如果同时存在则以第二种方式为准
 	
- Activity的Flags(标记位)	
 	- FLAG_ACTIVITY_SINGLE_TOP:Activity的SingleTop	启动模式 
 	- FLAG_ACTIVITY_NEW_TASK:Activity的SingleTask	启动模式 
 	- FLAG_ACTIVITY_CLEAR_TOP:同一个任务栈中所有位于它上面的Activity都要出栈，一般和FLAG_ACTIVITY_NEW_TASK一起使用，如果被启动的activity为standard模式则先连同启动的acivity一起出栈，在创建新的acticity实例放入栈顶
 	- FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS:等同android:excludeFromRecents="true"

#### 任务栈属性:
- 分类
  - 前台任务栈：
  - 后台任务栈：Acitivity位于暂停状态，用户可以通过切换将后台任务栈再次调到前台
- 属性
    - TaskAffinity:任务栈名字，Activity和application默认为应用包名
    - allowTaskReparenting(与字面理解相同，本属性允许activity重新指定Task。默认值是false)

	````
 	<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:taskAffinity=""
        android:allowTaskReparenting=""
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
	````
	````
	<activity 		android:name=".defaultsearch.DefaultSearchSuggestionActivity"
            android:taskAffinity=""
            android:allowTaskReparenting="">
            <intent-filter>
                <action android:name="android.intent.action.SEARCH" />
            </intent-filter>
	````


 ####   Activity的IntentFilter
- activity可以多个intentFilter
- action:一个intentFilter可以多个action，多个action只要匹配一个action就算匹配action成功。
- category：一个intentFilter可以多个category
- data：一个intentFilter可以多个data

````
<data android:scheme="string"
          android:host="string"
          android:port="string"
          android:path="string"
          android:pathPattern="string"
          android:pathPrefix="string"
          android:mimeType="string"/>
````
  - mimeType:指媒体类型，如:image/jpeg,audio/mpeg4-generic和video/*
  - URI:<scheme>://<host>:<post>/[<path>/<pathPrefix>/<pathPattern>]

 	  
- [demo代码](https://github.com/milin411/TestProject)