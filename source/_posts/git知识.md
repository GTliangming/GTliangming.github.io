---
title: git知识
date: 2020-09-08 16:00:58
tags: git
cover:
categories: git
---
###  git知识



#### 一、版本回退

```
使用  git  log 查看commit记录   附带参数：git log --oneline --graph 


HEAD表示当前版本   上一个版本就是HEAD^  上上个版本HEAD^^   往上100个版本写HEAD~100


git reset --hard  HEAD^ 版本回退至上一个commit

再次回到之前的版本
 git reset --hard  +commit的id（版本号）


命令git reflog用来记录你的每一次命令  包括add checkout  commit等 从中可以查看版本号


现在总结一下：
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
```

#### 二、管理修改

```
用   git diff HEAD -- 文件名    命令可以查看工作区和版本库里面最新版本的区别


如何丢弃工作区的修改 git checkout -- 文件名

用命令  git reset HEAD <file>    可以把暂存区的修改撤销掉（unstage）

小结
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区(git add)时，想丢弃修改，分两步，
    第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库(git commit)时，想要撤销本次提交，参考版本回退一节，
    不过前提是没有推送到远程库
```

#### 三、删除操作

```
先手动删除文件，然后使用git rm <file>和git add<file>效果是一样的
都是将本地的文件删除，然后也将版本库里面的文件删除

另一种情况是删错了，还想恢复回来，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
git checkout -- test.txt

git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
```

#### 四 远程仓库

```
本地仓库关联远程仓库
 git remote add origin 远程仓库地址
 添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
 
 我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
 还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

```

#### 五、分支管理

````
git merge 分支名    命令用于合并指定分支到当前分支
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，
能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并

小结
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>或者git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
````

#### 六、冲突解决

```
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
用git log --graph命令可以看到分支合并图。
```

#### 七、

```
1、Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

2、用git stash list命令看看

3、一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

4、另一种方式是用git stash pop，恢复的同时把stash内容也删了

5、你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令
 git stash apply stash@{0}
 
 Git专门提供了一个gitcherry-pick命令，让我们能复制一个特定的提交到当前分支：

小结
1、修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
2、当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；
3、在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，
    把bug提交的修改“复制”到当前分支，避免重复劳动。
```

#### 八、

````
开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D 分支名 强行删除。
````

#### 九、多人协作

```
要查看远程库的信息，用  git remote  或者，用git remote -v显示更详细的信息：
显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。


小结
1、查看远程库信息，使用git remote -v；
2、本地新建的分支如果不推送到远程，对其他人就是不可见的；
3、从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
4、在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，
    本地和远程分支的名称最好一致；
5、建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
6、从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。
```

#### 十、Rebase

```
rebase 做了什么操作呢？
首先， git 会把 feature1 分支里面的每个 commit 取消掉；
其次，把上面的操作临时保存成 patch 文件，存在 .git/rebase 目录下；
然后，把 feature1 分支更新到最新的 master 分支；
最后，把上面保存的 patch 文件应用到 feature1 分支上；


git rebase --continue
这样 git 会继续应用余下的 patch 补丁文件。


git rebase —abort
可以用 --abort 参数来终止 rebase 的行动，并且分支会回到 rebase 开始前的状态
```






