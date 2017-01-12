## Service全解析：[Service官方讲解](https://developer.android.google.cn/guide/components/services.html)
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
8. IntentService用于在单一子线程串行执行某些动作，等全部执行完了自行销毁，  
    使用中不用自己创建线程，只要提供一个空构造函数和onHandleIntent()的实现
9. 只有在第一个客户端绑定时，系统才会调用服务的`onBind()` 方法来检索 IBinder。  
    系统随后无需再次调用onBind()，便可将同一IBinder 传递至任何其他绑定的客户端。  
    故传递给Service的Intent参数，只有第一次绑定的Intent能被Service看到(在任意回调方法)
10. 已经绑定到某个Service的Activity，必须在finish之前调用unBindService()方法，  
    否则会造成Activity和ServiceConnection内存泄漏
11. 只有Activity、Service和Content Provider可以绑定到服务，无法从广播接收器绑定到服务。
12. 对于`onServiceDisconnected()`，只有在与服务的连接意外中断时（例如当服务崩溃或被终止时）  
    调用该方法。当客户端取消绑定时，系统不会调用该方法。
13. 对于同一个Activity和同一个ServiceConnection，连续调用bindService绑定同一个Service，  
    并不会有任何重复的效果，包括onBind()和onServiceConnected()都不会重复调用
14. 在bind系列回调方法中修改intent的extra值都是无效的，不会被后面的回调看到
15. 在Service一直处于启动状态没有销毁的情况下，onBind()和onUnbind()这对回调只会调用一次，  
    以后再次调用bindService()不会触发onBind()。  
    如果onUnbind()返回了false，则以后onBind()和onUnbind()任意一项都不会回调；  
    如果onUnbind()返回了true，则之后只会有onRebind()和onUnbind()这对，  
    且没有onUnbind()就不会有重复两次onRebind()，即也要配对出现
16. Service销毁条件：既不在bind状态又不在start状态  
    调用stopService()或stopSelf()只会取消start状态，调用unBindService()只会取消bind状态  
    这些方法都只能针对其中一个状态，对另一个状态无法控制