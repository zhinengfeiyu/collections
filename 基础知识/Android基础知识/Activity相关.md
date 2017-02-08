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
3. onDestroy()有两种情形会被调用，要么是调用finish()方法，要么是系统杀死或配置更改，  
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
9. 被启动的Activity获得前一个启动它的组件名字：getCallingActivity()，返回ComponentName。  
    必须requestCode>=才能有返回，否则返回null
10. isFinishing()通常用于在onPause()回调中判断Activity是不是要执行finish操作。  
    finish是一整个过程，在向系统发出finish()请求后会先将isFinished状态置为true，以便后续过程能及时看到  
11. recreate()方法用于重建当前Activity，与转屏后的操作类似，会走完生命周期到onDestroy()，
    再走onCreate()到onResume()。执行此方法时，屏幕会闪一下
12. 一个Activity可以从前面启动它的Activity上finish掉，而不是自己finish。  
    方法是finishActivity(int requestCode)。当然requestCode必须>=0。  
    因为不是自己finish掉的，onActivityResult()会有回调，但resultCode等返回数据设置无效，  
    是默认的RESULT_CANCELED。如果requestCode匹配多个Activity，就都会finish掉

#### Activity启动模式
1. singleTop模式下，Activity在栈顶启动自己的生命周期：  
    onPause -> onNewIntent -> onResume
2. singleTask模式下，Activity在栈顶启动自己的生命周期：  
        onPause -> onNewIntent -> onResume
3. singleTask模式下，同一个任务栈中，启动非栈顶的Activity：  
        onNewIntent -> onRestart -> onStart -> onResume
4. 虽然singleTask与FLAG_ACTIVITY_NEW_TASK意义相同，但是如果从Application启动Activity，只能使用后者。  
    此时如果没有指定taskAffinity属性，被启动的Activity使用默认的任务栈；  
    否则会在taskAffinity指定的任务栈启动。
5. 不在同一任务栈的两个Activity，界面切换会出现明显的不连续性，在系统任务列表会出现在不同的两组
6. 注意taskAffinity的命名，不能出现单独的单词，必须用点分隔的形式书写。如果不指定，默认与包名相同
7. taskAffinity要与singleTask模式(静态或动态)一起用才有效，否则即使是与包名相同也无效
8. Activity A启动standard模式的Activity B，B必定与A在同一个任务栈
9. 因为任务栈的存在，Activity按返回键退出的顺序不一定与startActivity顺序相同。  
    当前Activity所在的栈如果还有其他Activity，按返回键必定会先回到同栈的Activity。
10. singleInstance模式下，Activity会处在单独的一个任务栈中，该任务栈不允许其他Activity加入。  
    如果从该Activity启动一个standard模式的Activity，被启动的Activity会加入系统默认的任务栈。
11. AndroidManifest里声明的任何组件，如果要能被其他应用调用，必须**显式**地指定  
    exported属性为true，否则应用会因找不到组件而崩溃

#### 隐式启动Activity
1. 对于隐式启动Activity，如果没有找到匹配的IntentFilter，应用会直接崩溃；  
  如果找到多个匹配的IntentFilter，系统会弹出选择框，选择启动哪个Activity
2. 如果要隐式启动，Intent中的action必须要有，而且只能设置一个
3. IntentFilter中可以设置多个action。注意匹配时action字符串区分大小
4. 对于**所有的**隐式启动，系统**都会**给Intent自动加上`android.intent.category.DEFAULT`这个category，
  因而所有的IntentFilter必须**显式**地加上这个category才能匹配上
5. Intent可以设置多个category，IntentFilter也可以设置多个category。  
  要能成功匹配，Intent里的category集合必须是IntentFilter里category集合的子集
6. 如果要隐式启动其他应用的Activity，目标Activity不需要设置`exported=true`
7. 隐式启动前，判断能否找到匹配的Activity方法：  
  - `ComponentName c = intent.resolveActivity(getPackageManager())`，如果返回不为null则能找到
  - `getPackageManager().resolveActivity(...)`或`getPackageManager().queryIntentActivities(...)`
8. data和action匹配规则和定义规则类似，intent里只能指定一个data，IntentFilter里可以定义多个data
9. 如果一个Intent既有显示启动又有隐式启动代码，则**以显示启动代码为准**，忽略隐式启动的代码
