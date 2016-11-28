### 代码模板用于xml的情况：
1. applicable指示：xml text表示只能用于非标签处，即标签之间或文档首尾位置；要能在标签处使用，必须点选other

### 注意事项：
1. live template设置是全局通用的，即在一个项目的设置会应用到所有其他android studio项目
2. 打开代码模板设置窗口：File -> Settings -> Editor -> Live Templates。打开设置页面快捷键：CTRL+ALT+S

### 自带的代码模板：
1. Toast -> Toast.makeText(..).show();
2. const -> private static final int ... 
3. fbc -> (..)findViewById(..) 自带类型转换
4. logd,loge等 -> Log.d(TAG, "..")
