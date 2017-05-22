title: Android多国语言适配
date: 2015-04-17 23:04:29
tags:
categories: Android
---

##[Android多国语言适配](http://blog.csdn.net/dyllove98/article/details/8831908)
 - android语言信息都是读取strings.xml(项目/res/values/strings.xml)文件指定key的索引值。

 - android语言适配需要在-项目/res下新建不同的语言目录(文件命名规范:values-zh(国家)-rTW(r+区域))如:
       默认:项目/res/values
       中国台湾:项目/res/values-zh-rTW
       中国简体:项目/resvalues-zh-rCN

 - 用户启动APP默认读取跟android系统一样的语言文件，如没有配置各国语言则默认读取系统默认语言文件(项目/res/vlues/strings.xml)

 - 正在使用app怎么切换语言？
    <ol>
    <li>sharePreferences存入设置语言：</li>
    
	Sharences sharedPreferences = getActivity().getSharedPrefeivity().getPackageName(), 0);
	sharedPreferences.edit().putString("language", lanAtr).commit();
	
    <li>语言更新后，对于之前出现且目前仍旧存活的activity，语言设置是不生效的。可以通过重启对应的activity，让语言及时生效。
    </li>

     private void restart() {
              Intent it = new Intent(getActivity(), MainActivity.class); //MainActivity是你想要重启的activity
              it.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
             it.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
             getActivity().startActivity(it);
	   }
    
      </ol>
