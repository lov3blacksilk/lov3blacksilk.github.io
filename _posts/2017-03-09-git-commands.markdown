#GIT常用命令

1. 谁动了我的代码【重要命令】

``` 
find ./ -name [file_name] #查找文件路径
git blame [file_name]  #找到当时改动的提交commit id
相关配合命令：
git show [commit]   #显示本次提交的信息
git log --oneline [file_name] #显示修改历史
git show [commit] --diff-filter= [file_name] #显示本次提交的当前文件的改动信息
或者直接把commit id复制到gerrit上搜索当时的提交记录
```
使用场景： 当看到代码被别人改动过，但是又不知道当时为什么这样改时，使用这个命令可以看到当时修改的commit id，进而可以查找到具体的修改信息.

2. 创建分支【重要命令】
```
git checkout -t(--track) remote-branch  #创建一个跟踪远程分支的分支， 默认分支名为远程分支名
git checkout -b [branch_name] -t(--track) remote-branch #创建一个跟踪远程分支的分支， 分支名为branch_name
git checkout -b myfeature origin/myfeature #Git的新版本将启动自动追踪，如果你使用-b来checkout
```
使用场景：强烈推荐这种创建分支方法，如果不需要批量创建分支的话，使用repo start 创建分支有时会遇到未知的异常

重新创建分支也是暴力解决很多问题的关键，比如你在一个分支上的提交遇到了异常， 你可以先重新创建一个干净的分支，然后把刚刚的改动cherry-pick上去，删除有问题的分支然后再提交

我所已知的百分之九十以上的提交异常都可以用此方法解决。

3. 查看分支【常用命令】
```
git branch #查看本地分支
git branch -r #查看远程分支
git branch -a #查看本地和远程分支
git branch -vv #查看本地分支的远程分支指向
git branch --merged
git branch --no-merged
```
使用场景：高频使用

注：查看远程分支时如果显示的远程分支比较少，可以使用git pull/repo sync ./git fetch拉取远程分支信息

4. 查看日志【常用命令】
```
git log
git log -n #查看特定数量的分支
git log --oneline #单行输出日志
git log -p #可以这样依次查看单次提交变动的内容
git log --stat #可以同时看到文件变动的摘要
git log --author=Andy #如果你想根据指定的作者查找
git log --grep="Something in the message" #或者你可以搜索你提交信息的内容
git log feature/132 feature/145 ^master #如果你有feature/132 和ferature/145这两个分支，并想查看这些不在master上的分支内容。（ ^ 符号是意味着非）
```

5. 显示当前日志的具体信息【重要命令】

一般配合git log系列命令使用
```
git show [commit] #显示该次提交的详细具体信息
git show --name-only [commit] #显示该次提交的变动文件信息
git show [commit] --diff-filter= [file name] #显示该次提交的当前文件的具体改动信息
```
注：这三个都是常用命令

6. 删除分支【常用命令】
```
git branch -d(-D) #分别对应删除和强制删除
```

7. 从远程仓库更新代码【常用命令】
```
git pull #是fetch和merge的集合指令
repo sync . #会把最新的没有上传的提交给冲掉
```
注：一般在commit代码前先用git pull,保持本地版本库head和远程分支一致然后再提交

8. 暂存当前修改【重要命令】
```
git stash #将当前的改动暂存，并清空工作区
git stash pop #将暂存的最后一个改动弹出
git stash list #显示暂存列表
git stash apply #应用某一次暂存
```
注:常用指令，一般你在一个分支上的工作做到一半时需要切换到别的分支处理事情可以考虑使用此命令

9. 提交【常用命令】
```
git commit -m [commit] #直接提交
git commit -s #进入编辑模式，编辑完后再提交
git commit --amend #追加提交
```

10. 合并提交日志【重要命令】
```
git rebase -i HEAD~[number_of_commits]
git rebase -i [commit]
```
使用场景：主线分支与稳定分支的提交差了很多，但这时需要将这些提交都pick到稳定分支上，如果一个一个的pick过去的话会非常的麻烦，因为从主线往稳定版本提交本身就需要各种review，

使用这个命令的话只需要review一次即可。

11. 查看丢失的提交【关键时候非常有用】
```
git fsck --lost-found
git reflog
```
使用场景：误删了分支或者还没有将提交上传到服务器就给reset掉了。

12. 在git中忽略文件【常用命令】
```
.gitignore
```
使用场景：一般IDE为代码库生成的各种配置文件都需要忽略掉

13. 查看修改信息【常用命令】
```
git status #查看修改状态
git status -s #简化状态输出信息
```

14. 将工作区修改添加至暂存区【常用命令】
```
git add .
git add [file name]
git add [file src]
git add -p [file_name] #暂存一个文件的部分改动，这个命令不懂的话可以再单独问我
```

15. cherry-pick【重要命令】
```
git cherry-pick [commit_hash]
```
注：可以先使用git log [分支名] 查看某个分支的提交记录，然后把需要pick的记录pick到当前分支。

16. 回退命令【重要命令】
```
git reset --mixed|--hard|--soft [commit]
git reset HEAD #简化版的reset
git reset HEAD [file name] #仅仅回退一个文件
git checkout . #使用暂存区记录覆盖工作区
git checkout [file name] #使用暂存区记录覆盖工作区的一个文件
git rm file #从staging区移除文件,同时也移除出工作目录.
git rm --cached #从staging区移除文件,但留在工作目录中.
git rm --cached 从功能上等同于git reset HEAD,清除了缓存区,但不动工作目录树.
```
注:都是常用命令

17. 比较差异【重要命令】
```
git diff #比较工作去和版本库的差异
git diff --stage #比较暂存区和版本库的差异
```
注：还可以手动设置默认的版本比较工具

18. 使用补丁【重要命令，有时候使用有奇效】
```
git diff > *.patch
patch -p1 < *.patch
```
注：打补丁需要在同样的目录下。

使用场景：比如两个不同的库，但是是同一套代码，没办法在当前库下cherry-pick, 这时候可以先打成patch,然后到另一个代码库下在同样的目录层次下把补丁再打上去

19. 恢复失去的分支
```
git branch experimental SHA1_OF_HASH
```
你可以使用git reflog查看你最近访问过的SHA1数（版本号）

另一个方式就是使用 git fsck —lost-found ，悬空对象（dangling commit ）是就是失去HEAD指针的提交，（删除的分支只是失去了HEAD指针成为悬空对象）

20. 变基操作【重要命令】
```
git rebase
--rebase #不会产生合并的提交,它会将本地的所有提交临时保存为补丁(patch),放在”.git/rebase”目录中,然后将当前分支更新到最新的分支尖端,最后把保存的补丁应用到分支上.
rebase的过程中,也许会出现冲突,Git会停止rebase并让你解决冲突,在解决完冲突之后,用git add去更新这些内容,然后无需执行commit,只需要:
git rebase --continue就会继续打余下的补丁.
git rebase --abort将会终止rebase,当前分支将会回到rebase之前的状态.
```
注：可以选择在每次commit之前先git pull或者如果没有这样做的话在commit之后先git rebase在upload

21. 忽略追踪文件中的变更

如果你和你的同事操纵的是相同分支，那么很有可能需要频繁执行git merge或是git rebase。不过，这么做可能会重置一些与环境相关的配置文件，这样在每次合并后都需要修改。与之相反，你可以通过如下命令永久性地告诉Git不要管某个本地文件：
```
git update-index --assume-unchanged
```

22. 每隔X秒运行一次git pull

通常，合并冲突出现的原因在于你正在工作的本地仓库不再反映远程仓库的当前状态。这正是我们为什么每天早晨要首先执行一次git pull的缘故。此外，你还可以在后台通过脚本（或是使用GNU Screen）每隔X秒调用一次git pull：
```
screen
for((i=1;i<=10000;i+=1)); do sleep X && git pull; done
```

23. 清理

有时，Git会提示“untracked working tree files”会“overwritten by checkout”。造成这种情况的原因有很多。不过通常来说，我们可以使用如下命令来保持工作树的整洁，从而防止这种情况的发生：
```
git clean -f     # remove untracked files
git clean -fd    # remove untracked files/directories
git clean -nfd   # list all files/directories that would be removed
```

24. 将项目文件打成tar包，并且排除.git目录

有时，你需要将项目副本提供给无法访问GitHub仓库的外部成员。最简单的方式就是使用tar或zip来打包所有的项目文件。不过，如果不小心，隐藏的.git目录就会包含到tar文件中，这会导致文件体积变大；同时，如果里面的文件与接收者自己的Git仓库弄混了，那就更加令人头疼了。轻松的做法则是自动从tar文件中排除掉.git目录：
```
tar cJf .tar.xz / --exclude-vcs
```


