- TextView中可以使用gravity属性设置文字对齐方向，比如文字水平居中
- 对TextView设置点击事件，点击响应范围包括四周设置的drawable
- maxEms和maxLength属性的作用完全不同，maxEms是在TextView宽度为wrap_content时，对View的最大宽度进行限制，
    宽度不超过n个大写M字母宽度；maxLength才是限制setText的文本字符数，getText()的字符串可能不是setText()的字符串

### EditText
- EditText指定固定的高度后，提示文字及输入的文字默认竖直居中，需要额外指定gravity为top
- 调用setText()方法后，光标位置必定是移到头部；
    调用setTextKeepState()方法后，如果旧光标在第n个字符位置，新光标仍然在第n个字符位置，新文本不足n个字符，则移到最后

