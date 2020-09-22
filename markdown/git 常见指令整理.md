```

```

# git 常见指令整理

## 一、初始化与配置

```bash
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
git remote -v
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



## 二、提交和修改

```
git add filename/dirname   //将文件修改、新文件提交到工作区
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
git commit -m 'first commiit'     //提交更新，并注释信息“first commit”
```

```
git push -u origin master     //将本地项目更新到github项目上去
```



## 三、修改查看

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

git push //将本地的commit 提交到远端



## 四、分支管理

## 五、repo 的使用

## 六、其他命令

## 七、错误解决：

1、运行 git pull/push 时出现错误： 

`You are not currently on a branch. So I cannot use ...`

解决方法：`git pull -u origin master`  或者 `git push -u origin master` 



## GitHub 配置使用

1. 安装git
2. 在网页上创建git 仓库
3. 在本地下载；注意下载时不要用 http，要用 ssh 避免每次输入密码；
4. 注意，对应新的设备需要 ssh key:
   1.  ssh-keygen -t rsa -C  "2260797305@github.com" 
   2. 然后按3次回车；
   3. 在 ~/.ssh/ 目录下回生成一堆文件， id_rsa.pub 为公钥，加到 github 上即可，标题随意；
   4.  id_rsa  为私钥，不管；

