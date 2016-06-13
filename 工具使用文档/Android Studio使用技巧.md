IDE原来的快捷键：

文件操作：

- ALT+SHIFT+R <br>
    重命名某个文件，等同于右键某个文件->Refactor->Rename，两步操作换一步
- CTRL+SHIFT+C <br>
    复制文件在整个磁盘的绝对路径
- CTRL+ALT+SHIFT+C <br>
    复制文件在当前项目的路径，即在绝对路径的基础上，把“项目名/”及以前的部分全部去掉
- CTRL+F4 或 shift+单击窗口标题<br>
    关闭当前窗口
- CTRL+SHIFT+F4 <br>
    关闭所有窗口
    
文字编辑：

- CTRL+D <br>
    删除光标所在的行，并把光标置于下一行
- CTRL+SHIFT+DELETE <br>
    从光标处删除到行尾
- CTRL+DELETE <br>
    从光标处删除到单词尾部
- CTRL+BACKSPACE <br>
    从光标处删除到单词头部  
- SHIFT+ENTER <br>
    在光标处所在行的下面新增一空白行，并把光标移到新行的行首，省去了先把光标移到当前行尾的操作
- CTRL+ALT+ENTER <br>
    在光标处所在行的上面新增一空白行，并把光标移到新行的行首
- CTRL+/ <br>
    对每一行进行注释，既可以注释Java代码，也可以注释掉一段XML代码
- CTRL+SHIFT+/ <br>
    块注释，对Java和xml都有效
- CTRL+ALT+L <br>
    自动格式化代码
- CTRL+ALT+O <br>
    清除无效的import


非常有用的页面跳转：

- Alt+左箭头/右箭头 <br>
    返回/前进到上/下一个光标位置，在某个方法或类的调用处Ctrl+点击，跳转查看详情后，可以马上回到原调用处，还可以跨页面！
- CTRL+] <br>
    光标跳转到代码块尾部<br>
    对于左半括号[同上
    
    
- CTRL+ALT+S <br>
    打开设置界面
- CTRL+SHIFT+] <br>
    从光标处往后一直选中到当前代码块的末尾，即到"}"前面一个位置。<br>
    实际应用中，可以解决要使用某个代码块，找不到哪个"}"结尾的问题。
- ALT+SHIFT+X <br>
    运行APP或当前类（根据具体选择）
- ALT+SHIFT+D <br>
    调试APP或当前类（根据具体选择） <br><br><br>
- 双击SHIFT <br>
    全工程搜索
- CTRL+SHIFT+R <br>
    按文件名打开文件
- CTRL+E <br>
    打开最近浏览的文件
- 在XML硬编码的字符串或尺寸值光标处 ALT+ENTER 统一到strings.xml或dimens.xml
    

    
    
Android Studio的类列表中，对于不同类文件的标识：

- 对于接口，用绿色背景的"I"表示；
- 对于抽象类，左上角1/4圆用绿色表示；
- 对于final类，左上角用白点表示；
- 其他一般的类用蓝色背景"C"表示。

在打开一个文件的情况下，打开该文件所在的目录，ctrl+单击包名
