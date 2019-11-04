# Github 
```
git diff --staged //查看已经暂存(add)起来的变化，按下q退出对比;
git diff          //查看暂存前后的变化
```
## 提交更新
```
git add    //跟踪新文件
git	commit	-m “message”
git commit -a -m "message"  //跳过git add步骤，自动把所有已经跟踪过的文件暂存起来一并提交
```
## 添加远程仓库
```	
git remote add	<shortname>	<url> //添加远程仓库，并指定一个轻松引用的简写
git remote -v     //显示需要读写远程仓库使用的Git保存的简写与其对应的URL
```
## 推送到远程仓库
```
git	push [remote-name] [branch-name]
```

## 拉取远程服务器origin的master分支并且整合代码
```
git pull origin master //git pull相当于 git fetch 跟着一个 git merge FETCH_HEAD  ;如果发生了冲突，可以使用git reset --merge进行回退。
```

## 创建分支
```
git	branch [branch name] //仅仅创造一个分支，不会自动切换至新分支
```
git 有一个名为HEAD的特殊指针，指向当前所在的本地分支

## 分支切换
```
git checkout [branch name] //切换至已存在的分支
git log --oneline --decorate --graph --all //输出提交历史、各个分支的指向以及项目的分支分叉情况
```