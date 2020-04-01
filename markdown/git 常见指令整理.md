# git 常见指令整理

## 初始化与配置

```c
git init             //初始化 git 仓库
```

```text
git config --global core.autocrlf false //git设置关闭自动换行
```

```text
git config --global core.safecrlf true //避免window 和 Linux的换行差异
```

```text
//更换项目地址
git remote set-url origin git@192.168.6.70:res_dev_group/test.git 
git remote -v //查看当前仓库的地址；
```

```
git config core.filemode false  // 忽略文件权限的修改， 仅当前版本库
```

```
git config --global core.fileMode false // 忽略文件权限的修改，全局生效
```

```
git clone gitaddr   //从git 地址上抓取代码到本地；
```



## 提交和修改

```
git add filename/dirname   //将文件修改、新文件提交到缓冲区
```

```
git rm filename // 取消对file 的跟踪，同时删除本地文件
```

```
git rm  dirname -r // 取消对文件夹的跟踪，同时删除本地文件夹
```

```
git reset filename // 取消对某个文件的提交。
```

```
git checkout filename // 从服务器恢复某一文件，注意会覆盖本地的修改
```

```
sudo find /tmp -name "*.pyc" | xargs rm -rf // 移除工程中所有的*pyc 文件
```

```
//回退到某一版本
git reset --hard 248cba8e77231601d1189e3576dc096c8986ae51 
```

```
git commit -m 'first commiit'     //提交更新到本地仓库，并注释信息“first commit”
```

```
git commit -am "cjfojwq" //如果没有文件修改，直接git commit 会出错；所以加上 -a;
```

```
git push -u origin master     //将本地仓库的更新提交到远端仓库上
```

```
git reset --soft HEAD^1  //取消本地仓库的提交，但是缓冲区和工作修改的内容不会有影响； HEAD^1 指上一次的修改；
git reset --soft HEAD~1  //windows的bash
```

```
git pull // == git fetch + git merge
```

```
git checkout a.c //从缓冲区或者版本库中获取；先缓存区，如果没有就从本地库中；即，如果本地修改已经add 到缓冲区中了，使用该指令是无法恢复到 本地版本库中的；
git checkout HEAD  a.c //强制从本地版本库中恢复a.c；如果 a.c 有提交到缓冲区，那么缓冲区的该记录会被清除掉；

```



## 修改查看

```
git log .  //查看当前目录下的提交记录
```

```
git status . // 查看当前目录下的修改
```

```
git status . -uno  //忽略未追踪的文件
```

```
git diff --name-only <commit-1> <commit-2> //获取两次提交修改的文件名。
```

```
git diff commit_id1  commit_id2 //对比两次修改的差异，包括具体内容
```

```
git diff	            工作区 vs 暂存区
git diff head	        工作区 vs 版本库
git diff –cached        暂存区 vs 版本库
```

```
git show commit-id  # 查看某次提交的详细更改
```

```
git diff --name-only HEAD~ HEAD  //获取最近一些修改的文件
```

```
git log --pretty=oneline filename   //查看某个文件的修改历史（仅显示commit id）
```

```
git log -- filename   //查看某个文件的修改历史记录，不涉及修改的细节，会显示提交作者，日期等等信息
```

```
git log -p filename   //查看某个文件的修改历史记录，并显示diff
```

```
git show commit-id filename //查看某次提交中某个文件的diff
```

```
git log commit-id --stat //查看某次提交的修改的文件名，不涉及具体的内容修改；
```

```
git pull // 从远端更新最新代码，如果有冲突会报错，不用担心覆盖本地修改
```

```
git push //将本地的commit 提交到远端
```

```
git log --stat -2 //查看最近两次提交修改的文件名
```

## git tag

```
git tag //列出已有的tag
```

```
git tag v1.0 //创建一个叫 V1.0 的tag
```

```
git tag -a v1.0 //创建一个叫 V1.0 的tag，-a 表示加上备注，可以直接 -m 跟上备注，或者是直接回车，会有一个窗口让你添加备注；
```

```
git show tag_name //查看一个tag，如果有备注，会显示出备注信息；
```

```
git tag -a v1.2 9fceb02 -m "my tag" //给某个指定commit id 打上 tag，默认是当前位置打；
```

```
git push origin v1.0 // 将本地tag v1.0 上传到服务器；所有git pull 更新会同时更新tag;
```

```
git push origin --tags //将本地所有tag 上传到服务器；
```

```
git tag -d v0.1.2  //本地删除tag v0.1.2
```

```
git push origin :refs/tags/<tagName> //删除服务器上的tag，测试未成功；
```

## git stash

```
git stash  //将工作区和缓冲区的修改暂存到缓存区，并将工作区和缓冲区覆盖为本地服务器的代码；
```

```
git stash save // 同git stash
```

```
git stash pop //回复最近的一次 stash将暂存的code 回复到工作区；会自动merge；同时将 stash 的记录删除；
```

```
git stash list //查看当前stash 保存情况；
```

```
git stash apply //与 git stash pop 类似，但是不会将 stash 记录删除；
```

```
git stash apply stash@{0} //恢复指定的stash 存储的记录；不会删除记录；
```

```
git stash drop //删除指定的记录；
```

```
git stash clear //清空stash 中的所有内容；
```



## 分支管理

```
git branch //查看所有的本地分支；*所在的分支即为当前分支
```

```
git branch -r //查看所有的远程分支；
```

```
git branch -a //查看所有的本地远程分支；
```

```
git checkout 分支名字 //切换分支；当前的分支的工作区和缓冲区如果有修改，会带到新的分支去；
```

```
git switch 分支名字 //同git checkout 分支名字
```



## repo 的使用

## 其他命令

## 错误解决：

1、运行 git pull/push 时出现错误： 

`You are not currently on a branch. So I cannot use ...`

解决方法：`git pull -u origin master`  或者 `git push -u origin master` 