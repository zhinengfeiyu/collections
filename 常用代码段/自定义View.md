如果自定义的组件继承自View，而不是其他现成的Widget，最好重写onMeasure()方法。

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    
        int desiredWidth = 100;
        int desiredHeight = 100;
    
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
    
        int width;
        int height;
    
        //Measure Width
        if (widthMode == MeasureSpec.EXACTLY) {
            //Must be this size
            width = widthSize;
        } else if (widthMode == MeasureSpec.AT_MOST) {
            //Can't be bigger than...
            width = Math.min(desiredWidth, widthSize);
        } else {
            //Be whatever you want
            width = desiredWidth;
        }
    
        //Measure Height
        if (heightMode == MeasureSpec.EXACTLY) {
            //Must be this size
            height = heightSize;
        } else if (heightMode == MeasureSpec.AT_MOST) {
            //Can't be bigger than...
            height = Math.min(desiredHeight, heightSize);
        } else {
            //Be whatever you want
            height = desiredHeight;
        }
    
        //MUST CALL THIS
        setMeasuredDimension(width, height);
    }

在代码中设置某个尺寸，且尺寸的单位不是px：

    TextView text = new TextView(getActivity());
    int padding = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
            4, getActivity().getResources().getDisplayMetrics());
    text.setPadding(padding, padding, padding, padding);