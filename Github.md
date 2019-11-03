# Github 
```
git diff --staged //查看已经暂存(add)起来的变化，按下q退出对比;
git diff          //查看暂存前后的变化
```
## 提交更新
```
git	commit	-m “ 第一次提交”
```
## 添加远程仓库
```	
git	remote add	<shortname>	<url> //添加远程仓库，并指定一个轻松引用的简写
git remote -v     //显示需要读写远程仓库使用的Git保存的简写与其对应的URL
```
## 推送到远程仓库
```
git	push [remote-name] [branch-name]
```