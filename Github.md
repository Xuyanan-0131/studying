

## Github_studing 
```
git diff --staged //查看已经暂存(add)起来的变化，按下q退出对比;
git diff          //查看暂存前后的变化
```
### 提交更新
```
git add    //跟踪新文件
git commit -m “message”
git commit -am "message"  //跳过git add步骤，自动把所有已经跟踪过的文件暂存起来一并提交
```
### 添加远程仓库
```	
git remote add <shortname> <url> //添加远程仓库，并指定一个轻松引用的简写
git remote -v     //显示需要读写远程仓库使用的Git保存的简写与其对应的URL
```
### 推送到远程仓库
```
git push [remote-name] [branch-name]
```

### 拉取远程服务器origin的master分支并且整合代码
```
git pull origin master //git pull相当于 git fetch 跟着一个 git merge FETCH_HEAD  ;如果发生了冲突，可以使用git reset --merge进行回退。
```

### 创建分支
```
git branch [branch name] //仅仅创造一个分支，不会自动切换至新分支
```
git 有一个名为HEAD的特殊指针，指向当前所在的本地分支

### 分支切换
```
git checkout [branch name] //切换至已存在的分支
git log --oneline --decorate --graph --all //输出提交历史、各个分支的指向以及项目的分支分叉情况
git checkout -b [branch name] //新建一个分支并同时切换到那个分支上,是git branch 和git checkout的简写
```

### 分支合并
```
git checkout master         //切换至master分支
git merge [branch name]     //将branchname的分支与master合并，其实是将master的指针移动到了branchname的指针
git branch -d [branch name] //删除分支

git merge --abort           //若合并失败，尝试恢复到你运行合并前的状态，中断合并
git merge -Xignore-space-change [branchname] //忽略所有空白修改 
```

### 删除提交历史
```
尝试 运行 git checkout --orphan latest_branch
添加所有文件git add -A
提交更改git commit -am "commit message"
删除分支git branch -D master
将当前分支重命名git branch -m master
最后，强制更新存储库。git push -f origin master
```
参考：https://blog.csdn.net/love_dl_forever/article/details/79380921

### 指令
```
git diff                //查看修改过的内容
git reset --hard HEAD^  //回退到上个版本（上个提交的commit）HEAD^^回到上上个版本，HEAD~10回退到10个版本之前
git reflog              //查看命令历史
git checkout -- [file]  //丢弃工作区的修改，回到上次add 或者commit的状态

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

git merge --no-ff -m "merge with no-ff" [branch]        //禁止fast-forward

git stash                   //存储当前工作
git stash list
git stash pop
git cherry-pick <commit>   //复制一个特定的提交到当前分支

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

git branch --set-upstream-to=origin/dev dev //建立本地分支和远程分支的关联
git checkout -b dev origin/dev      //创建远程origin的dev分支到本地

git gc --prune=now                  //
git pull origin dev2:dev2           //从远端拉取本地不存在的分支
```

```
git rebase []  //与cherr-pick <commite>本质相同
git push origin --delete [远端分支]
git branch  -a //获取远端分支
git push origin --delete dev  //删除远端分支
```
