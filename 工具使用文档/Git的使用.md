参考链接：[Git使用教程-涂根华的博客](http://www.cnblogs.com/tugenhua0707/p/4050072.html)

### Shell通用命令：
命令关键字 --help：查看帮助信息
小技巧：在输入时按下tab键可以自动补全，连按两下tab可以显示待补全列表
##### 查看命令(不改变任何东西的命令)：
1. ls命令：
    - ls：查看当前路径下的文件夹和文件，只显示最简略的信息，跟窗口显示看到的情况一样，不会显示上级和下级的信息，  
            也不会显示.git类型的隐藏文件夹。
    - ls -la：显示当前能到达的所有路径，包括往上能到达的路径，包括创建时间信息。其中，./代表当前目录，../代表上级目录
    - ls a*b：显示当前目录下所有以a开头的文件以b结尾的文件，*代表1个或1个以上字符，可以出现在任意位置，可以出现多次
    - ls -l a*b：同上，但是会显示详细信息
2. cat命令：
    - cat 文件名：查看文件内容。文件名两边可以加单引号或双引号，如果文件名带空格，必须加上引号。查看目标不能是文件夹
    - cat -n 文件名：同上，只是显示内容的时候加上了行号
3. cd命令：
    - cd ..：回到上一级目录
    - cd ../..：回到上两级目录。三级以上类推

##### 复制命令：
1. cp命令：
    - cp 待复制文件 目标文件夹：该命令无法复制文件夹
    - cp -a 待复制文件夹 目标文件夹：递归地把一个文件夹及其包含的文件复制到另一个目录下，如果目标已存在，则只复制没有的文件  
                                    如果文件内容有改动，则覆盖目标文件内容

##### 移动命令：
1. mv命令：
    - mv 待移动文件或文件夹 目标文件夹

##### 重命名命令：
1. mv命令：
    -mv 待命名的文件或文件夹 新名字

##### 新建命令：
1. touch命令：
    - touch 文件名：新建文件。不能是文件夹
2. mkdir命令：
    - mkdir 文件夹名：新建文件夹

##### 删除命令：
1. rm命令：
    - rm 文件名：强制删除文件，不能删除文件夹
    - rm -rf 文件名或文件夹名：强制删除文件或文件夹
2. rmdir命令：
    - rmdir 文件夹：删除空文件夹，如果文件夹有下级文件或文件夹，则无法删除

##### 内容输入命令：
1. echo命令：
    - echo "abc">>aa.txt：添加文字"abc"到aa文件里。注意，该方式不能用"\n"换行。多次执行echo会自动换行
    - echo "abc">aa.txt：同上，但是会先删除原来的内容
    - echo >>aa.txt：加入一个换行
    - echo -e "aa\nbb"：允许用"\n"转义换行
2. cat命令：
    - cat <<'eof'>> aa.txt(去掉引号)：多行输入，最后输入eof结束输入，不输按enter即换行
    - cat <<'eof'> aa.txt(去掉引号)：同上，不过是覆盖原输入

### Git命令：
##### git本地查看命令
- git log：查看commit历史，包括时间、作者、版本号、commit备注
- git log 提交名：从指定提交处开始回溯，因而git log等同于git log head
- git log --pretty=oneline：每条提交在一行显示，只显示提交名和commit备注
- git log --stat：列举时加上更改的文件以及每个文件有多少行发生改动
- git log 提交名1..提交名2：列举从指定两个提交之间的所有提交(包括)，并且提交1应早于提交2
- git log -数字 提交名：输出限制为指定数量个提交，比如-2即只会输出前2个
- git log --graph：以提交图的形式显示
- head^=head~1 head^^=head~2 以此类推
- ORIG_HEAD：在merge和reset操作之后，会在移动head的同时，把先前版本的head记录到orig_head中
- git rev-parse 标签/相对提交名/简写：把任何形式的提交名转换为绝对提交名
- git show (提交名)：查看提交的详细信息，包括diff的信息。如果没有版本号，则查看head
- git diff：显示工作目录和索引之间的差异，与已经commit的东西无关，未追踪的文件不会参与比较
- git diff 提交名：显示工作目录和指定提交间的差异，与索引无关，未追踪的文件不会参与比较
- git diff --cached (提交名)：显示索引和指定提交的差异，省略提交名则默认为head
- git diff 版本号1 版本号2：比较差异，注意是把前一个作为旧版，后一个作为新版，二者交换书写顺序会导致不同的比较结果

##### git本地修改命令
- git rm 文件名：从版本库删除一个文件，与git add正好相对，同时工作目录里该文件也会删除。  
                该命令与在工作目录删文件后再add一样效果
- git mv 文件名1 文件名2：同上，重命名文件，效果等同于先执行git rm后执行git add
- git reset --hard 提交名：head指向、索引、工作目录内容全部改为与指定提交一致的状态，未追踪的文件除外
- git reset (--mixed) 提交名 (文件名)：与工作目录无关，head指向、索引改为与指定提交一致的状态，可用于取消刚暂存的东西
- git reset --soft 提交名：仅修改head指向，索引和工作区不变，可用于修改commit信息
- git commit --amend：等同于先执行git reset --soft，再commit。可用于修改提交信息，或者改动后再次提交，  
                      但是会直接替换原来的head处提交，而不是新建提交
- git checkout -b 新分支名 起始提交处：从起始提交处拉出一个新分支，起始提交处默认是head，实际操作可以改为head^等                      


##### git服务端交互命令
- git remote rm origin：删除关联的远程库
- git config --global alias.st status / git config --global alias.psm 'push origin master' 指定除"git"之外的单词或单词组合的别名
- git config --unset --global user.email/alias.st：与上面的命令相对，移除设置

- git reset --hard 版本号：指定往前或往后穿越到任意一个版本，版本号通过git reflog查看
- git reset --hard HEAD^/HEAD^^：回退到上个版本/上上个版本
- git reflog：查看版本操作历史，显示缩略版本号、commit备注
- git checkout ./-- 不带引号的文件名：撤销工作区修改。如果存在add但没有commit的内容，则回到add后的状态，删除没有add的修改；如果没有add的内容，则回到最近一次commit完的状态
- git checkout -- 文件名：在commit之前执行，可恢复删除的文件
- git branch -a：查看本地和服务器分支，服务器分支用红色标出
- git branch 分支名：创建新分支，但不切换过去，如果当前已存在该分支则报错
- git checkout 分支名：切换到指定分支，如果该分支不存在则报错
- git checkout -：切换回上一分支。注意，多次执行该命令只会在两个分支之间来回切换，即当前分支和上次操作的分支，类似于循环执行后退和前进命令
- git checkout -b 分支名：创建新分支并切换到该分支，相当于上面两条命令先后执行，如果分支已存在则报错，并不会切换过去
- git branch -d 分支名：删除指定分支，不能在本分支上删除当前所在的分支，必须在其他分支上删除
- git branch -D 分支名：同上，但是改命令为强行删除分支，即使还没合并
- git merge 分支名：使当前所在分支内容和merge后面指定的分支内容一致，也就是修改当前所在的分支，接着删除merge后指定的分支也可以
- git remote (-v)：查看远程库信息，加上-v查看详细信息
- git push origin :待删除的远程分支 / git push --delete origin 待删除的远程分支： 删除掉远程分支

### adb命令：[adb使用官方链接](https://developer.android.google.cn/studio/command-line/adb.html)
1. adb devices：显示所有连接的设备及其状态
2. adb install apk路径：安装电脑上的apk到手机
3. adb push 电脑文件路径 手机文件路径：复制电脑文件到手机
4. adb pull 手机文件路径 电脑文件路径：复制手机文件到电脑。注意路径分隔符，手机路径是/，windows路径是双反斜杠。  
    同时目标是自己取的文件名，而不是文件夹
5. adb shell：进入远程shell命令，直接操控手机目录，按exit退出远程shell
