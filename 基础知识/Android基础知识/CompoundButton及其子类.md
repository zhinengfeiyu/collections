- 点击一个CompoundButton时，会先触发onCheckedChanged()回调，后触发RadioButton的onClick()回调
- CompoundButton仅在普通按钮的基础上加了一个功能：可以切换开和关两种状态，并且点击时可以自动切换
- CompoundButton是一个抽象类，其实现类有：CheckBox, RadioButton, Switch, ToggleButton以及各自的兼容类
- CompoundButton的`toggle()`方法，完全等同于`setChecked(!isChecked())`方法，除了RadioButton
    仅允许关闭状态执行
- 在`onCheckedChanged()`回调里再次调用`setChecked(boolean)`方法，会触发开关状态改变及UI刷新，
    但不会递归执行`onCheckedChanged()`回调
- `setChecked(boolean)`方法，如果传进去的状态跟当前状态一样，不会做任何操作
- CompoundButton的button属性，指定的drawable并不会影响View的宽高测算，只会在绘制的时候把该drawable绘制到
    指定的地方。关于绘制的位置，不会考虑padding，一定是居左的，只有上中下需要根据gravity来决定。
    该属性对应`setButtonDrawable()`方法
- 针对button属性，还有配套的buttonTint和buttonTintMode属性
- 实现类CheckBox只是指定了一个默认样式，其他啥都没做，就跟TextView的子类Button一样
    
#### 关于RadioGroup和RadioButton
- 实现类RadioButton可以脱离RadioGroup单独使用，跟CheckBox的区别，一是默认样式，
    二是点击操作仅允许打开，而不允许关闭，除非代码里执行`setChecked()`方法
- RadioGroup在xml中特有的属性只有1个: "checkedButton"属性，指定初始打开的RadioButton的id
- RadioGroup由于继承自LinearLayout，包括orientation等父类属性都能正常使用
- RadioGroup里面可以放任何类型的View，不过会把直接子View里类型为RadioButton的单独拎出来处理
- RadioGroup内如果有多个RadioButton的checked属性为true，只打开最后一个加入布局的RadioButton
- 点击同一个RadioButton多次，或者多次setChecked同一个布尔值多次，只要没改变开关状态，就不会触发RadioButton
    的onCheckChanged监听，也不会触发RadioGroup的onCheckChanged监听
- 当一个RadioButton的开关打开时，会同时触发自己的监听和另一个已打开的RadioButton监听，以及RadioGroup的监听，
    其中RadioGroup的监听里的checkedId参数是新打开的RadioButton的id
- RadioGroup里可以一个都不选中，只要调clearCheck()或者check(-1)，这是一种默认状态，
    这时同样会触发RadioGroup的onCheckChanged回调，传进去的id是-1