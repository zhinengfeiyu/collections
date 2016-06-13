AdapterView是一个抽象类，直接继承自ViewGroup类。但是有好几个ViewGroup方法不能用于AdapterView，包括：

- 各种参数的addView方法；
- removeView, removeViewAt, removeAllViews方法；
- setOnClickListener()方法。

下面将很可能会用到的方法总结如下：

- getAdapter(),setAdapter()方法，获得/设置与AdapterView绑定的适配器对象，基本是必须设置的；
- getCount()方法，返回与AdapterView绑定的数据对象的条目数；
- getEmptyView(),setEmptyView()方法，获得/设置当没有数据为空时替代显示的View；
- getFirstVisiblePosition(),getLastVisiblePosition方法，获得第一个/最后一个可见列表项在数据集中的索引；
- getItemAtPosition()方法，返回类型为Object，返回该索引处的数据对象，在Adapter的getItem方法重写的返回值；
- getItemIdAtPosition()方法，返回long，同上；
- get/setOnItemClickListener()，需重写onItemClick (AdapterView<?> parent, View view, int position, long id)；
- get/setOnItemLongClickListener()，同上；	
- get/setOnItemSelectedListener()，待定；
- performItemClick(View,position,id)，自动执行一次点击动作；
- getSelectedView,getSelectedItem,getSelectedItemPosition,getSelectedItemId
- setSelection设置选中


关于AdapterView中用到的Adapter

[android中Baseadapter的 getItem 和 getItemId 的作用和重写](http://www.2cto.com/kf/201405/299601.html)<br>
[ListView之BaseAdapter的基本使用以及ViewHolder模式](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1021/1815.html)

[BaseAdapter重写模板](E:\Android\MyCodes\MyLaboratory\DemoCollection\AdapterView.md)
