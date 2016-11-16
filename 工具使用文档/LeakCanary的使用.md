### [LeakCanary官方说明](https://github.com/square/leakcanary)

##### LeakCanary内存检测工具使用步骤：  

第一步，在app目录下的build.gradle的最下面dependencies{}里面添加如下语句（不是最外面的build.gradle）：  

    debugCompile 'com.squareup.leakcanary:leakcanary-android:1.4-beta2'
    releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.4-beta2'
    testCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.4-beta2'
    
  并点击同步代码  
  
第二步，新建一个Application，应用使用这个Application（在<application>标签下的name属性设置为该自定义的Application）。  
    重写该Application的onCreate方法，代码如下：
    
    public class HSApplication extends Application {
        @Override
        public void onCreate() {
            super.onCreate();
            LeakCanary.install(this);
        }
    }
