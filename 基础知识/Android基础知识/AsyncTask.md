- AsyncTask处理执行结果的线程可以不是主线程，而是任意线程，只要在构造函数里面传入想使用的线程对应的Handler/Looper就行，
    不过传进来的Handler不会直接使用，而是根据Looper新建一个Handler。
    如果判断是UI线程，则使用所有AsyncTask公用的绑定UI线程的Handler
- AsyncTask如果没有在execute()方法指定线程池，会使用全局公用的线程池，而且每次execute的任务会按调用顺序依次执行，类似单线程
- AsyncTask有一个静态的execute(Runnable)方法，该静态调用不会把结果返回给UI线程，只是在子线程执行一个任务，
    需要注意的是这里使用的只能是上面提到的类似单线程的线程池
- 可以调用静态方法，指定所有AsyncTask公用的默认线程池，即以后不再使用类似单线程的线程池
- 每个AsyncTask对象，只能调用一次execute方法，再次调用会崩溃，因为内部有判断执行状态
- AsyncTask需指定3个泛型，对于使用不到的泛型，写Void类
- AsyncTask的cancel()方法，只是在内部改了一个标志位，需要在doInBackground实现方法中及时判断该标志位，
    把后台执行方法提前return掉，cancel()方法才能起到及时取消的效果
- doInBackground()方法执行过程中，手动调用publishProgress()方法，才会触发onProgressUpdate()回调
- 内部切换执行线程，使用的是Handler发消息的方式，onProgressUpdate()和onPostExecute()都是在handlrMessage里面调用的，
    因为发送的消息体里面带有AsyncTask对象这个obj，就能成功执行到AsyncTask内部的回调方法
- onPreExecute()这个回调其实完全可以不用，因为该回调并不是等任务真正开始执行前调用，而是在调用execute()方法时立即调用
- 默认使用的线程池，核心线程数设置为最少2个，最多4个，最佳是CPU内核数-1；并且核心线程允许有30s的超时回收