- 创建仓库 : `mkdir 目录名`、`cd 目录名`
- 查看远程库：`git remote`,加参数`-v`产看更加清楚的信息
1. 初始化Git仓库 : `git init`

2. 添加文件到Git仓库

    i. `git add<file>`,可以反复多次使用，添加多个文件

    ii.`git commit -m` 使用`-m`后加推送版本的注释（不加会后悔的，至少我是）
3. 掌握工作区的状态 ： `git status`
4. 查看修改的内容 ：`git diff` 
5. 查看以往修改的内容：`git log`,可以使用参数：`--pretty=onelne`
6. 回退版本：`git reset --hard HEAD^`,上一个版本号用：`HEAD^`,上上一个版本号是：`HEAD^^`，也可以使用版本号回退。
7. 查看命令历史，可方便回退未来的版本：`git reflog`
8. 放弃工作区的修改：`git checkout --file`
9. 撤销暂存区修改：`git reset HEAD file(文件名)`
10. 删除：`git rm test.txt`
11. 关联远程的GitHub:`git remote add origin git@github.com:自己的GitHub名/自己的git目录名.git`
12. 推送本地库内容到远程库：`git push  origin master`
   如果加了`-u``git push -u origin master`参数的话就是一次性推送master分支所有的内容，简单来说就是把master文件夹所有的东西都提交上去。


---
##### 分支
1. 创建分支：`git checkout -b dev分支名` 
   `-b`参数表示创建并替换，相当于：` git branch dev`  `git checkout dev`
2. 查看当前分支：`git branch`，此命令会列出所有的分支，在当前分支的前面加一个*
3. 切换分支：`git checkout master(分支名)`
4. 合并分支：`git merge dev`
5. 删除分支：`git branch -d dev(分支名)`
   以上未master分支并没有修改数据，可以如此快速合并分支。下面为不能快速合并分支的情况：
6. 不能快速合并的时候，需要手动修改文件，再重新提交
7. 查看分支合并情况：`git log --graph --pretty=oneline --abbrev-commit`
8. 普通模式合并分支：合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
9. 查看本地分支和远程分支的关联关系`git branch -v` 
10. 暂存工作区的内容：`git stash`
11. 查看暂存的内容：`git stash list`
12. 恢复暂存区的内,但不删除stash的内容：`git stash apply `
13. 恢复暂存区的内,删除stash的内容：`git stash pop`
14. 删除stash的内容:`git stash drop`
15. 丢弃一个没有被合并过的分支：`git branch -D <name>`
16. 推送本地分支到远程仓库：` git push origin branch_name`
17. 本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致


---
#### 标签：
1. 在当前分支上创建标签：`git tag <name>`
2. 查看所有的标签：`git tag`按照字母排序
3. 查看标签信息：`git show <tagname>`
4. 创建带有说明的标签：`git tag -a v0.1 -m "..." 版本号`
5. 删除标签：`git tag -d 标签`
6. 推送某个标签到远程，使用命令`git push origin <tagname`
7. 一次性推送全部尚未推送到远程的本地标签：`git push origin --tags`
8. 删除远程的标签：`git tag -d v0.9`先删除本地的标签，再`git push origin :refs/tags/版本号 `
---
部分遇见过的问题：

###### 1. 推送时遇到：

`error: Cannot pull with rebase: You have unstaged changes.` 
`error: Additionally, your index contains uncommitted changes. `

是告诉你有些东西你没有提交，要先提交
解决办法：
-  `git stash`把未提交的内容先暂存起来
-  `git pull --rebase`拉取远程代码
-  `git push` 提交代码
-  `git stash pop stash`再继续原来的内容，再把stash的内容删了

###### 2. 关联github时提示：`fatal: remote origin already exists.`
-  先输入`git remote rm origin`

-  再输入`git remote add origin git@github.com:自己的地址` 就不会报错了！

-  如果输入`git remote rm origin` 还是报错的话，`error: Could not remove config section 'remote.origin'.` 我们需要修改gitconfig文件的内容

-  找到你的github的安装路径,找到一个名为gitconfig的文件，打开它把里面的`[remote "origin"]`那一行删掉就好了！
##### 3.github查看中文乱码

- 将文件用Notepad++打开后把文件转为`utf-8 无 BOM编码`重新上传就好了

##### 4.`git pull`提示`no tracking information`，

则说明本地分支和远程分支的链接关系没有创建，使用：`git branch --set-upstream-to=origin/<branch> dev`

##### 5.Git操作的过程中突然显示被占用

`Another git process semms to be running in this repository, e.g. an editor opened by ‘git commit’. Please make sure all processes are terminated then try again. If it still fails, a git process remove the file manually to continue… `
翻译过来就是git被另外一个程序占用，重启机器也不能够解决。

原因在于Git在使用过程中遭遇了奔溃，部分被上锁资源没有被释放导致的。

解决方案：进入项目文件夹下的 .git文件中（需要显示隐藏文件夹）删除index.lock文件即可。

##### 6.git push有几个可选值

nothing, current, upstream, simple, matching

其用途分别为：

- **nothing** - push操作无效，除非显式指定远程分支，例如`git push origin develop`（我觉得。。。可以给那些不愿学git的同事配上此项）。
- **current** - push当前分支到远程同名分支，如果远程同名分支不存在则自动创建同名分支。
- **upstream** - push当前分支到它的upstream分支上（这一项其实用于经常从本地分支push/pull到同一远程仓库的情景，这种模式叫做central workflow）。
- **simple** - simple和upstream是相似的，只有一点不同，simple必须保证本地分支和它的远程
  upstream分支同名，否则会拒绝push操作。
- **matching** - push所有本地和远程两端都存在的同名分支。


##### 7.git pull 失败 提示：`fatal: refusing to merge unrelated histories`

在进行git pull 时，添加一个可选项`git pull origin master --allow-unrelated-histories`,强行合并分支















