title: tablayout方法setupWithViewPager()坑
date: 2016-01-29  10:30:00
tags:
- tablayout
- Chrome
- 坑

categories: Android
---



#### tablayout方法setupWithViewPager()坑 - 吴明
- tallayout平常使用setupWithViewPager()

````
ViewPager viewPager=(ViewPager)findViewById(R.id.view_pager);
TabLayout tabContainView = (TabLayout) findViewById(R.id.pick_school_category_contain);
viewPager.setAdatper(new FragmentStatePagerAdapter(FragmentManager,fragments));
tabContainView. setupWithViewPager(viewPager);
````
- 查看setupWithViewPager()方法源码

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
- 就会发现这里有三个坑
	- setTabsFromPagerAdapter
	
	````
	public void setTabsFromPagerAdapter(@NonNull PagerAdapter adapter) {
        removeAllTabs();
        for (int i = 0, count = adapter.getCount(); i < count; i++) {
            addTab(newTab().setText(adapter.getPageTitle(i)));
        }
    }
    
	````
	removeAllTabs()这个就是说把前面所有tablayout添加的view都删掉。也就是说在之前不管怎么处理view都被干掉。然后设置为PagerAdapter返回的title
	
	- setOnTabSelectedListener(new ViewPagerOnTabSelectedListener(viewPager));
	
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
		这里主要不算不算太坑，最主要的是设置了点击tablayot默认是viewpager是滚动的，自己可以设置这个时间监听。重写他的方法
	- selectTab(getTabAt(curItem));
	
	````
	if (adapter.getCount() > 0) {
            final int curItem = viewPager.getCurrentItem();
            if (getSelectedTabPosition() != curItem) {
                selectTab(getTabAt(curItem));
            }
        }
	````
	这里最主要是第一次默认选中第一个。相对应一些viewpager第一次就选中的不是第一个，这个就是一个很大的一个问题就是相当于viewpager点击了两次。
	- 分享这个坑主要是给大家提前填。以后还是不能偷懒就为了省代码而忘了看封装的方法的代码是不是有问题。
	
