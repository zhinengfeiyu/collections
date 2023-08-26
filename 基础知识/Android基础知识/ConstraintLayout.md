# ConstraintLayout使用细节整理

- 内部控件@id或者@+id是一样的效果，即使@id对应的控件在后面也能正确识别
- 在chains链设置为spread，各控件均分空间之前，目标方向上的子控件间margin会先预留出来，相当于一个Space控件先占位，不计入子控件占用的空间
- 在chains链设置为spread且有子控件设置了weight之后，未设置weight的子控件默认使用的weight是0，即不参与自动延伸；如果所有控件均未设置weight，所有控件的weight为1即均分剩余空间
- 当一个控件设置为gone时，会连带着周围的margin一起缩为一个点，即一起让出空间，但是其他控件还可以参照它正常设置位置
- 仅在chains链的所有控件的所有长度都不为0时，才会自动延伸之间间距，否则只要有一个控件的自身长度为0，就变为自动拉伸控件本身，且仅拉伸长度为0的那些控件，之间间距不自动拉伸，此时weight属性仅对长度为0的控件有效
- 当控件A相对控件B顶部和底部对齐，但是高度比B大，这时A仍然会与B居中对齐，即使A的边界可能超出外层ConstraintLayout，此时会被截断
- 控件A参照控件B设置margin，这个margin值必须设置为控件A的属性，不能设置在被参照的控件B上，否则会失效
- bias属性0.0和1.0在xml中可以写成0和1，效果是一样的
- layout_width="wrap_content"+constraintedWidth="true"，等价于layout_width="0dp"+constraintWidth_default="wrap"，
    如果不加上述约束只写layoutWidth="wrap_content"，那么当内容过长后四周设置的约束条件就不一定满足，内容展示也会出现被截断的情况
- GuideLine的orientation属性horizontal表示横线，vertical表示竖线
- GuideLine的宽高无论设置成什么值都是一样的效果，测量的时候视作0
- GuideLine设置四周的margin无效，会忽略该属性
- Placeholder有两个专属的属性app:content和app:placeholder_emptyVisibility，后者表示没有内容的时候的可见性，区分是否占用空间，
    此时visible和invisible是一样的效果
- Layer作用是给多个View设置共同的背景，根据各个View的宽高注意要写在所有被设置的View之前，有用的属性就是 app:constraint_referenced_ids，
    android:backgound和android:padding，由于它本质是一个View，可以当成一个ViewGroup增加点击事件，也可以被上面的View拦截。
    Layer的宽高以及margin或者Constraint约束属性都没有作用，宽高及位置完全取决于顶层的那些view
- ImageFilterView可以用来实现圆角，圆角大小可以用绝对数值或者0-1的百分比，用百分比表示圆角大小等于图片宽高的较小值的一半乘以该比例。
    该组件可以用于其他类型容器，不只是ConstraintLayout
- ImageFilterView可以用来实现图片之上的蒙层，altSrc表示覆盖在上层的另一张图片，可以是颜色值引用，crossfade默认是0，表示altSrc图片的透明度
