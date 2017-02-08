## ContentProvider使用细节：[官方链接](https://developer.android.google.cn/guide/topics/providers/content-provider-basics.html)
1. 在AndroidManifest中声明的provider不需要指定exported=true，直接可以给其他应用使用
2. 同一应用中**使用**provider，不需要去申请获取指定的权限
