title: tablayout方法setupWithViewPager()坑
date: 2016-01-29  10:30:00
tags:
- tablayout
- Chrome
- 坑

categories: Android
---



为什么说慎用了？(最新TalLayout源码有改动，讨论只限之前的源码)
先看TabLayout平常怎么使用setupWithViewPager()

````
ViewPager viewPager=(ViewPager)findViewById(R.id.view_pager);
TabLayout tabContainView = (TabLayout) findViewById(R.id.pick_school_category_contain);
viewPager.setAdatper(new FragmentStatePagerAdapter(FragmentManager,fragments));
tabContainView. setupWithViewPager(viewPager);
````
运行没有什么问题，但是深入看代码的你就会发现一些问题。
查看setupWithViewPager()方法源码

````
 public void setupWithViewPager(@NonNull ViewPager viewPager) {
        final PagerAdapter adapter = viewPager.getAdapter();
        if (adapter == null) {
            throw new IllegalArgumentException("ViewPager does not have a PagerAdapter set");
        }

        // First we'll add Tabs, using the adapter's page titles
        setTabsFromPagerAdapter(adapter);

        // Now we'll add our page change listener to the ViewPager
        viewPager.addOnPageChangeListener(new TabLayoutOnPageChangeListener(this));

        // Now we'll add a tab selected listener to set ViewPager's current item
        setOnTabSelectedListener(new ViewPagerOnTabSelectedListener(viewPager));

        // Make sure we reflect the currently set ViewPager item
        if (adapter.getCount() > 0) {
            final int curItem = viewPager.getCurrentItem();
            if (getSelectedTabPosition() != curItem) {
                selectTab(getTabAt(curItem));
            }
        }
    }
````
获取ViewPager的Adapter后使用了三个方法
```
setTabsFromPagerAdapter(adapter);
setOnTabSelectedListener(new ViewPagerOnTabSelectedListener(viewPager));
//这个处理逻辑我们简单理解放在一个方法处理
if (adapter.getCount() > 0) {
            final int curItem = viewPager.getCurrentItem();
            if (getSelectedTabPosition() != curItem) {
                selectTab(getTabAt(curItem));
            }
        }
```
###setTabsFromPagerAdapter
首先看第一个方法实现
````
public void setTabsFromPagerAdapter(@NonNull PagerAdapter adapter) {
        removeAllTabs();
        for (int i = 0, count = adapter.getCount(); i < count; i++) {
            addTab(newTab().setText(adapter.getPageTitle(i)));
        }
    }
````
removeAllTabs()这个就是说把前面所有TabLayout添加的view都删掉,并都设置view的title。
>我觉的有问题：1.从逻辑来讲前面都是通过判断adapter为空在进行下一步操作，那就是说adapter必须先不为null才能执行逻辑代码。可是我们已经为ViewPager初始化设置了adapter那还有必要这里重复需要初始化adapter？

###setOnTabSelectedListener(new ViewPagerOnTabSelectedListener(viewPager));
	
````
		public static class ViewPagerOnTabSelectedListener implements TabLayout.OnTabSelectedListener {
        private final ViewPager mViewPager;

        public ViewPagerOnTabSelectedListener(ViewPager viewPager) {
            mViewPager = viewPager;
        }
        @Override
        public void onTabSelected(TabLayout.Tab tab) {
            mViewPager.setCurrentItem(tab.getPosition());
        }
        @Override
        public void onTabUnselected(TabLayout.Tab tab) {
            // No-op
        }

        @Override
        public void onTabReselected(TabLayout.Tab tab) {
            // No-op
        }
    }
  ````
> 这里本身没有太多问题，就是如果已经设置了 监听，点击tab不是滚动的，这里重新设置就会设置为滚动了
### selectTab(getTabAt(curItem));
````
	if (adapter.getCount() > 0) {
            final int curItem = viewPager.getCurrentItem();
            if (getSelectedTabPosition() != curItem) {
                selectTab(getTabAt(curItem));
            }
        }
````
>前面方法setTabsFromPagerAdapter都初始化清空了，肯定getCurrentItem()=0啊，这里还需要设置吗？难道还考虑多线程？
###总结
总结TabLayout.setupWithViewPager（）设计很不合理。

ps：其实主要的是提醒自己以后不要光api调用而不去研究具体api做了什么。
	
