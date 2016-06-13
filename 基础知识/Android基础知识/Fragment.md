[Fragment官方介绍](http://developer.android.com/intl/zh-cn/guide/components/fragments.html)

Fragment与Activity的生命周期关系：<br>
刚打开Activity：<br>
    Fragment onAttach > Fragment onCreate > Fragment onCreateView <br>
    > Activity onCreate > Fragment onActivityCreated <br>
    > Activity onStart > Fragment onStart > <br>
    Activity onResume > Fragment onResume <br>
关闭Activity：<br>
    Fragment onPause > Activity onPause <br>
    > Fragment onStop > Activity onStop <br>
    > Fragment onDestroyView > Fragment onDestroy > Fragment onDetach > Activity onDestroy <br>
进入下一个Activity：<br>
    Fragment onPause > Activity onPause <br>
    > Fragment onStop > Activity onStop <br>
返回到当前Activity：<br>
    Activity onRestart > Activity onStart > Fragment onStart <br>
    > Activity onResume > Fragment onResume <br>
添加Fragment：<br>
    onCreate > onCreateView > onActivityCreated > onStart > onResume <br>
Fragment添加后是否加入返回栈，生命周期都一样，pop即完全移除到onDetach <br>
移除Fragment（未添加到返回栈）：<br>
    onPause > onStop > onDestroyView > onDestroy > onDetach <br>
移除Fragment（添加到返回栈）：<br>
    onPause > onStop > onDestroyView <br>
使用popBackStack恢复移除的Fragment：<br>
    onCreateView > onActivityCreated > onStart > onResume <br>

    
    
写在xml中的静态Fragment，移除后虽然变成null，但视觉上并不会消失；只有在代码中动态添加的Fragment，移除后才会真正消失

FragmentTransaction调用addToBackStack(null)可以在commit之前任何时候，包括在实施增删改Fragment之前调用，之后popBackStack的效果都是一样的