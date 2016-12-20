
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


##### 源码阅读记录（View）
1. 调用setOnClickListener方法时，系统会自动将clickable变量置为true
2. 在xml中声明onClick属性，需要在对应的Context(Activity)或其父类中声明对应的public void 任意方法名(View v){..}，  
  其中访问权限必须是public；参数类型必须是View；方法名可以任取，但是必须与xml中一致。  
  不过如果以上条件不满足，xml中会自动提示，如果正确的话，Activity和xml之间是可以通过ctrl单击跳转的。  
  但是系统是通过反射实现的，第一次调用onClick会从传给View的Context开始解析，如果当前Context没有，会去baseContext寻找；  
  找到method后，每次调onClick方法都会直接反射调用method的invoke方法。
3. layoutDirection属性指定布局是从左往右还是从右往左，有LTR、RTL、INHERIT(从父布局继承，默认值)、LOCALE
4. 没有paddingStart/paddingEnd属性的情况下，当padding属性定义后，上下左右边距肯定是一样的，另外定义的paddingLeft等失效

##### 源码阅读记录（ViewGroup）
1. OnHierarchyChangeListener这个内部类可以用来监听子View被添加或删除事件
2. ViewGroup.MarginLayoutParams继承自ViewGroup.LayoutParams，多出了四个方向的margin值以及marginStart,marginEnd。  
    xml中，当设置了margin属性时，marginLeft等单方向的属性都会失效。
    
##### 源码阅读记录（LinearLayout）
1. LinearLayout.LayoutParams继承自ViewGroup.MarginLayoutParams，多出了weight和gravity两个属性，  
    对应于xml里的layout_weight和layout_gravity
2. 在layout_weight>0的情况下，API23及以上会忽略layout_width/layout_height，API23以下会考虑宽/高属性，并加入到总的剩余空间中
3. 当LinearLayout中设置了gravity，子View中设置了layout_gravity，则以layout_gravity为准

##### 源码阅读记录（LayoutInflater）
1. 基本语法：View v = inflater.inflate(xml资源, root, attachToRoot)
2. 如果xml资源的根标签是merge，则root必须不为null，attachToRoot必须不为false；其他根标签对后2个参数无限制。  
    以下规则只对非merge根标签适用：
3. 如果root!=null且attachToRoot==false，则返回的view是xml对应的控件。  
    此时root唯一的作用是给该view设置LayoutParams，具体地说，root唯一的作用是让view生成指定类型的LayoutParams，  
    此时root的实际类型(不是声明类型！)才是有用的，root里面具体有哪些属性并没有作用。  
    根据root类型的不同，生成的LayoutParams类型也不同，比如root是LinearLayout，则设置进view的是LinearLayout.LayoutParams，  
    此时view的xml属性除了默认的layout_width,layout_height以外，所有margin属性和layout_gravity,layout_weight也会被设置进去。
4. 如果root==null，此时attachToRoot无论是什么值都不会被系统考虑。同上，返回的view也是xml对应的控件。  
    不同的是，此时view的LayoutParams属性是null，因为不会根据layout_XXX属性去生成。
5. 如果root!=null且attachToRoot==true，则除了会根据root生成对应类型的LayoutParams(规则同3)以外，还会调用root.addView方法，  
    根据该LayoutParams将view添加到root里面。此时返回结果不再是xml对应控件，而是addView之后的root。
6. inflater.inflate(R.layout.XXX, root)等同于inflater.inflate(R.layout.XXX, root, root!=null)
7. merge必须是根标签，include不能是根标签
8. View的onFinishInflate回调时机：从XML解析完该View对应的标签，以及该标签里面的所有子元素，  
    并把子元素对应的子View全部addView进来，然后父View把该View addView进来(父View的其他子View可能还没解析)。  
    上面操作都完成会执行该回调
9. requestFocus标签应作为某个View的子标签，作用是使得该View调用requestFocus()方法。  
    requestFocus标签里面所有的深层次东西都会忽略，因为解析过程中会直接跳到requestFocus的结束标签
10. 除了几个特殊的不直接表示控件的标签，以控件名字命名的直接表示控件的标签，必须包含layout_width和layout_height属性，  
    否则映射过程中会抛异常
11. 关于include标签：
    - 如果include进来的layout文件是以merge作为根标签，则会原原本本地把该layout插入到include位置。  
        原来的include标签其他属性都会失效，不能重写任何东西。
    - 如果include进来的layout文件不以merge作为根标签，则有可能会根据include标签的部分属性作出覆盖。规则如下：
    - 对于layout_XXX系列的属性，即用于生成对应LayoutParams的属性，解析过程中优先使用include标签的layout_XXX。  
      如果include标签里缺失layout_width或layout_height，即生成LayoutParams失败，再去被导入的layout文件找。
    - 系统选择layout_XXX时会全部使用include标签的或被导入的文件的，不可能出现交叉使用的情况。  
        即，要么全部用include属性里的layout_XXX，要么全部用被导入文件的layout_XXX。
    - include标签里的id属性和visibility属性会直接覆盖被导入文件的属性，如果include标签有定义的话
    - 除了以上的layout属性，layout_XXX属性，id属性和visibility属性，其他属性在include标签里写了都没有效果