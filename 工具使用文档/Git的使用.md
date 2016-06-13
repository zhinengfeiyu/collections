参考链接：[Git使用教程-涂根华的博客](http://www.cnblogs.com/tugenhua0707/p/4050072.html)

- pwd命令：显示当前的目录
- git init：把当前目录变成git可以管理的仓库
- git diff 文件名：查看修改了什么内容
- git log：查看commit历史，包括时间、作者、版本号、commit备注
- git log --pretty=oneline：查看commit历史，只显示时间和commit备注
- git reset --hard 版本号：指定往前或往后穿越到任意一个版本，版本号通过git reflog查看
- git reflog：查看版本操作历史，显示缩略版本号、commit备注
- git checkout ./-- 不带引号的文件名：撤销工作区修改。如果存在add但没有commit的内容，则回到add后的状态，删除没有add的修改；如果没有add的内容，则回到最近一次commit完的状态
- git checkout -- 文件名：在commit之前执行，可恢复删除的文件
- cat 文件名：查看文件内容
- git branch：查看当前所有分支，当前所在的分支前面加*
- git branch 分支名：创建新分支，但不切换过去，如果当前已存在该分支则报错
- git checkout 分支名：切换到指定分支，如果该分支不存在则报错
- git checkout -b 分支名：创建新分支并切换到该分支，相当于上面两条命令先后执行，如果分支已存在则报错，并不会切换过去
- git branch -d 分支名：删除指定分支，必须在本分支上删除当前所在的分支，必须在其他分支上删除
- git merge 分支名：使当前所在分支内容和merge后面指定的分支内容一致，也就是修改当前所在的分支，接着删除merge后指定的分支也可以
- git remote (-v)：查看远程库信息，加上-v查看详细信息

命令退不出来，按ctrl+C <br>
输入前面输过的命令，按PgUp/PgDn
