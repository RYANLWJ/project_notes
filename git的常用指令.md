# Git的一些常用语法 


****

### 1. git init 初始化一个仓库
```
    $ git init
```
### 2. git add . 将当前的所有文件添加索引(添加到暂存区)
```
    $ git add .
```
### 3. git commit -m "这里写注释" (提交到本地仓库)

```
    $ git commit . -m "a commit message"
```

### 4. git push 上传到远程仓库(github)


```
    $ git remote rm origin  //删除添加的远程地址(options)
    $ git remote add origin https://github.com/RYANLWJxxx(仓库名).git //添加远程仓库地址
    $ git pull // 从另一个存储库或本地分支获取并集成(整合(options)
    $ git push -u origin master //推送到远程仓库
```
- ! 如果不是初次推送到远程仓库的话,需要先把文件pull下来然后再push上去

### 5. git log 查看所有版本
```
    $ git log
```
### 6. git reset 恢复到想恢复的版本
```
    $ git reset --hard  42afbbb3(版本号) //恢复42afbbb3..这个版本
    $ git push -f //执行推送
```
### 7. 查看分支
```
    $ git branch
```
### 8. 创建本地分支
```
    $ git branch XXX //XXX为分支名称
```
### 9. 切换本地分支
```
    $ git checkout XXX //XXX为分支名称
```
### 10. 合并本地分支
```
    $ git checkout master //切换带主分支
    $ git merge branch //合并分支
```
### 11. 把本地分支上传到github
```
    $ git push -u origin water //water 为你创建的分支名
```
### 12. 查看两个版本的差别
```
    $ git diff 版本1 版本2
```
### 13. 查看远程仓库历史版本
```
    $ git log remotes/origin/master //master为远程分支名
```
## 合并远程仓库的两个分支

1. 把源码clone到本地库中（master分支）。
```
    $ git clone [gitsite  git远程网址]
```
2. 在本地新建一个与远程的dev版本相同(被合并的版本)的dev分支
```
    $ git checkout -b dev origin/dev
```
3. 返回到master版本
```
    $ git checkout master
```
4. 把本地的dev合并到master
```
    $ git merge dev
```
5. 把本地的master同步到远程
```
    $ git push origin master
```

<br/>


## 本地内容修改了未commit就pull了远程内容下来造成的冲突



> **error**: Your local changes to 'c/environ.c' would be overwritten by merge.  Aborting.
Please, commit your changes or stash them before you can merge.
这个意思是说更新下来的内容和本地修改的内容有冲突，先提交你的改变或者先将本地修改暂时存储起来。处理的方式非常简单，主要是使用git stash命令进行处理，分成以下几个步骤进行处理。

1. 先将本地修改存储起来

```
$ git stash
这样本地的所有修改就都被暂时存储起来 。
$ git stash list//可以看到保存的信息：

    git stash暂存修改
    git stash暂存修改

其中stash@{0}就是刚才保存的标记。
```

2. pull内容

```
$ git pull//暂存了本地修改之后，就可以pull了。
```

3. 还原暂存的内容

```
$ git stash pop stash@{0}
系统提示如下类似的信息：
Auto-merging c/environ.c
CONFLICT (content): Merge conflict in c/environ.c
意思就是系统自动合并修改的内容，但是其中有冲突，需要解决其中的冲突。
```

4. 解决文件中冲突的的部分

```
打开冲突的文件，会看到类似如下的内容：

git冲突内容
git冲突内容
其中Updated upstream 和=====之间的内容就是pull下来的内容，====和stashed changes之间的内容就是本地修改的内容。碰到这种情况，git也不知道哪行内容是需要的，所以要自行确定需要的内容。
解决完成之后，就可以正常的提交了。
```
<br/>


## git恢复本地删除的文件夹取消增加的文件
####  git项目中有时候会在本地增加或者删除了一些文件或者文件夹，但是又不想提交，一般情况下，我们取消本地所有修改：

```
    $ git checkout .//取消指定文件修改：

    $ git checkout  filename//取消指定文件删除：

    $ git checkout  filename//恢复到上一个版本，则可以解决整个文件夹删除的修改：

    $ git reset --hard HEAD^　//取消本地增加的文件和所有修改：

    $ git checkout . && git clean -df
```