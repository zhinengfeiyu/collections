#### Bundle源码阅读记录
1. Bundle用于存放多个key-value对，其中key必须是String，value必须是Parcelable

#### Activity源码阅读记录
1. 在onCreate回调里可以直接调用finish()，这样会立即走onDestroy()回调，跳过了中间的onStart()等生命周期
2. 从Activity A启动Activity B，会等到A的onPause()回调结束，才会创建B，所以onPause()执行时间不能过长  
   前一个Activity的onStop()回调在后一个Activity完全展示，即onResume()回调完成之后才调用  
   onStop()在Activity完全对用户不可见时调用。  
   综上，从Activity1启动Activity2的正常回调流程：  
        Activity1:onPause()->Activity2:onCreate()->Activity2:onResume()->Activity1:onStop()  
   如果启动Activity2时快速返回，可能产生如下流程：  
        Activity1:onPause()->Activity2:onCreate() onResume()->Activity1:onResume()，  
        没有onStop()和onRestart()
3. onDestroy()有两种情形会被调用，要么是调用finish()方法，要么是系统杀死以腾出内存，  
    可以用isFinishing()来区分这两种情形。  
    按返回键退出界面，isFinishing()返回true；转屏触发onDestroy()，isFinishing()返回false。  
    onDestroy()回调不应该用来保存数据，而应该用来回收资源，比如结束线程。
4. startActivityForResult的requestCode参数如果小于0，则返回时不会调用onActivityResult()方法，  
    即使finish()前调用了setResult()方法。如果requestCode>=0，则必定会调用onActivityResult()。  
    startActivity(intent)等同于startActivityForResult(intent, -1)。  
    resultCode可以是任意整数，如果finish()前没有调用setResult()方法，则resultCode默认为RESULT_CANCELED
5. 获取当前contentView的方法：  
        View v = ((FrameLayout) findViewById(android.R.id.content)).getChildAt(0)
6. 调用setContentView()或addContentView()必然会触发onContentChanged()回调，且只有这两个方法会触发
7. setContentView会先移除android.R.id.content内的所有组件，再执行addView操作;  
    而addContentView()不会先移除，直接执行addView()，因外层是FrameLayout，会直接覆盖在原布局上面
8. startActivities(Intent[])可以一次性启动多个Activity，顺序按照数组声明的顺序，  
    视觉上是直接启动最后一个Activity