- AppBarLayout在回调onOffsetChanged()方法中，offset参数在完全展开时为0，收缩时为负值
- AppBarLayout用于实现特效的子View，可以是任意类型的View，而不局限于ToolBar，特效通过在子View里面加layout_scrollFlags参数实现。
    属性值scroll表示上滑时该View与下面的滚动布局同步上滑，看上去像下面滚动布局的一部分；
    属性值enterAlways表示下滑时，该View可以比下面滚动布局先滑出来，而不用等滚动布局滚到头才出来(只有scroll参数的情况)；
    属性值snap表示在停止滚动时，如果该View部分可见，则自动滑动到完全可见或完全不可见；
    如果不指定layout_scrollFlags参数，则表示固定显示，不会消失。因AppBarLayout继承自LinearLayout，
    如果有多个子View，不带参数的子View应该放在最下面
- 对于SwipeRefreshLayout，无论是通过手势还是直接代码设置refreshing状态为true，
    在取消refreshing状态之前，也就是只要还在转圈过程中，都无法响应refresh手势操作