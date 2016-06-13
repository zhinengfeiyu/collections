
一个View如果没有父布局，那么layout_width、layout_height无论设置成什么值都不起作用，包括具体值。

如果一个View的enabled值为false，就无法响应onTouch事件。




<br><br><br><br>
关于自定义View

如果自定义的组件继承自View，而不是其他现成的Widget，最好[重写onMeasure()方法](E:\Android\MyCodes\MyLaboratory\DemoCollection\自定义View.md)。

自定义属性的类型：string , integer , float, dimension , reference , color , enum , flag , fraction ， boolean<br>
如果一个属性可以是多种类型，类型间加"|"，例：<attr name="XXX" format="reference|color" />

xmlns:toolbar=http://schemas.android.com/apk/res/cn.zzm.toolbar <br>
注意，“toolbar”可以换成其他的任何名字，后面的url地址必须最后一部分必须用上自定义组件的包名。

自定义View如果是内部类，全名写成：com.xxx.xxx.OuterClass$InnerClass




