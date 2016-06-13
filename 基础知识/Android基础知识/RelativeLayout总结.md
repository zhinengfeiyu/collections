RelativeLayout总结

使用"@id/…"时，所写的id必须在上文中已经定义，不能使用在下文定义的id

RelativeLayout的子控件属性总结——

1.    按照控件之间常规的上下左右依次排列：（指定控件ID）
	- layout_above 指定在某控件上面
	- layout_below 指定在某控件下面
	- layout_toLeftOf 指定在某控件左边
	- layout_toRightOf 指定在某控件右边
2.    按照控件之间某条边对应对齐：（指定控件ID）<br>
	注：设置下列属性后，再设置对应方向的margin值，这个值将以指定控件的对齐线为基准计算，而不是父布局
	- layout_alignBaseline 两控件的文字基线对齐
	- layout_alignTop 两控件上边线对齐
	- layout_alignBottom 两控件下边线对齐
	- layout_alignLeft 两控件左边线对齐
	- layout_alignRight 两控件右边线对齐
3.    按照在父布局中的位置：（指定true/false）
	- layout_alignParentTop 上边线与父布局上边线对齐
	- layout_alignParentBottom 下边线与父布局下边线对齐
	- layout_alignParentLeft 左边线与父布局左边线对齐
	- layout_alignParentRight 右边线与父布局右边线对齐
	- layout_centerInParent 在父布局中间
	- layout_centerHorizontal 在父布局水平方向的中间
	- layout_centerVertical 在父布局垂直方向的中间
4.    其他：（指定true/false）
	- layout_alignWithParentIfMissing 如果锚点控件找不到，以父布局作为目标
