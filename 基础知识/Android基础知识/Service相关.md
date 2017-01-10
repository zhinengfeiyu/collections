## Service全解析
1. onCreate(),onDestroy()均不需要重写父类，因为父类该回调为空实现
2. 关于onStartCommand()回调的第三个参数startId，在service创建完成后，  
    第一次调onStartCommand，startId为1，第二次调用为2，以此类推；  
    销毁再创建后，startId又会从1开始
3. 每次调用onStartCommand，系统会记录下最新的startId；
    调用`stopSelf(int)`时，会将传递的int值与<strong>最新的</strong>startId比较，
    只有二者相同，才能真正销毁service。  
    `stopSelfResult(int)`与之相同，唯一的区别是，如果因startId不一致销毁失败，会返回false，
    否则返回true。  
    不带int的`stopSelf()`虽然调用了stopSelf(-1)，但这里的-1不会与startId比较，
    该方法必定会立即销毁service
4. stopSelf()操作因为不是UI操作，可以在子线程执行而不需要用Handler
5. onStartCommand()的返回值有3个，在系统因内存不足杀死该service后才起作用：
    - START_STICKY：super的返回值，内存充足后会重建该service，但上次传进的Intent变为null；
    - START_NOT_STICKY：内存充足后不会重建该service
    - START_REDELIVER_INTENT：内存充足后会重建该service，且Intent是最近传递的那个
6. 在AndroidManifest注册的Service实现类，必须要有<strong>空参数</strong>的构造函数，  
    但不注册的Service实现类可以只有带参数的构造函数，像IntentService
7. 待启动的Service如果没有在AndroidManifest注册，运行时不会崩溃，只是启动不了而已