# git使用方法

## 首先初始化，打开git bash，设置你的name和email：  
```bash
git config --global user.name "yourname"
```

```bash
git config --global user.email "youremail@xxx.com"
```



## 现有文件加入git仓库
右键Git Bash进入到项目文件夹

输入`git init`(使当前文件夹加入git管理）

输入`git add .` .代表全部文件，可以指定文件名称将其内容添加到git中

输入`git commit -m "初始版本"`（双引号内容自己更改）

输入`git remote add origin https://github.com/xxxxxx/xxxxxx.git`xx为你自己的https地址，连接你的guthub仓库。

输入`git push -u origin master`上传项目到Github。这里会要求输入Github的账号密码，按要求输入就可以。



## 从git上下载项目
命令行进入到需要下载项目文件夹的位置，输入
```bash
git clone https://github.com/xxxxxx/xxxxxx.git
```
clone完之后cd进入到项目文件夹即可执行git操作



## 正常修改流程
先pull仓库最新文件到本地（但不会覆盖本地已有文件），再修改文件，修改完文件之后可以先`git status`查看一下修改的文件，红色字modified为未添加到暂存区的文件，之后`git add`当前文件添加到暂存区
之后`git add .`将所有修改文件添加到暂存区，再使用`git commit -m "第二次修改"`添加注释信息并提交到本地仓库,最后再push提交。


## git fetch与git pull区别
git fetch 相当于是从远程获取最新到本地，不会自动merge.  
git pull：相当于是从远程获取最新版本并merge到本地.




## git命令常用操作
#### 查看历史记录
```
git log
```

#### 查看分支
查看本地分支：
```
git branch
``` 
查看远程分支：
```
git branch -r
```

#### 创建分支
```bash
git branch [name]
```

#### 切换分支
```bash
git checkout [name]
```

#### 推送创建的分支
```bash
git push origin [name]
```

#### 将本地分支与远程分支关联之后才能直接执行git push推送
```
git push origin [name] -u
```
此代码是将当前branch push到origin的branch,-u表示同时建立关联，以后再推送到远程只需git push



#### 推送分支
```
git push origin [name]
```

#### 删除分支
```
git branch -d [name] 
```
`-d`选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用`-D`选项


#### 删除某条commit

`git log`查找要删除的前一次提交的 `commit_id` ，将 `commit_id` 替换成复制的值

```
`git rebase -i commit_id`
```
进入 Vim 编辑模式后，按`i`进入编辑模式，将要删除的 commit 前面的 `pick` 改成 `drop`，编辑完成后点击Esc后，再输入`:wq`保存退出


#### 删除远程分支
```
git push origin -d [name]
```



#### 查询当前远程的版本
```
git remote -v
```


#### 从远程拉取代码到本地
```
git pull [remoteName] [localBranchName]
```


#### 撤销git pull合并返回任意commit
运行`git reflog`命令查看你的历史变更记录
```
$ git reflog
e52e52e (HEAD -> master, origin/master, origin/HEAD) HEAD@{0}: pull: Merge made by the 'recursive' strategy.
5738b64 HEAD@{1}: commit: 初始版本
fe91322 HEAD@{2}: clone: from https://raolight@bitbucket.org/jiudu/dns_mobile.git
```
返回初始版本
```
git reset --hard 5738b64
```



#### 查看暂存区和工作目录的状态
```
git status
```

#### 查看本地仓库文件
```
git ls-files
```

#### 合并某分支到当前分支
```
git merge [name]
```


##### 撤销合并恢复到以前状态
```
git reset --hard HEAD
```

##### 撤销已经提交的
```
git reset --hard ORIG_HEAD
```



## 多人开发
#### 开发分支（dev）上的代码达到上线的标准后，要合并到 master 分支
```bash
git checkout dev #切换到dev分支
git pull #拉取最新版本
git checkout master #切换到master分支
git merge dev #将dev分支合并到当前分支(当前为master)
git push -u origin master #推送当前分支(当前为master)到远程master分支
```

#### 当master代码改动了，需要更新到分支（dev）上
```bash
git checkout master #切换到master分支
git pull #拉取最新版本
git checkout dev #切换到dev分支
git merge master #将master分支合并到当前分支(当前为dev)
git push -u origin dev #推送当前分支(当前为dev)到远程dev分支
```

#### 强制提交本地分支覆盖远程分支（慎用）
```
git push origin branchname --force
```

#### 强制拉取远程覆盖本地分支（慎用）
```bash
git fetch --all
git reset --hard origin/master
git pull
```

## Git提交记住用户名和密码
#### 永久记住密码
```
git config --global credential.helper store
```
会在`C:\Users\用户名`目录的.gitconfig文件中生成下面的配置。
```bash
[credential]
    helper = store
```
如果没有`--global`，则在当前项目下的`.git/config`文件中添加。

#### 临时记住密码
默认记住15分钟：
```
git config –global credential.helper cache
```
下面是自定义配置记住1小时：
```bash
git config credential.helper 'cache –timeout=3600'
```





#### 撤销做出的修改
git如何删除本地所有未提交的更改，包括修改的、新增的、删除的，还有一些编译生成的临时文件。就是回到上一版本的干净状态。查了下有两个相关的命令：
```
git clean -df
```
```
git reset --hard
```
有些需要注意的问题是第一个命令只删除所有untracked的文件，如果文件已经被tracked, 修改过的文件不会被回退。而第二个命令只把tracked的文件revert到前一个版本，对于untracked的文件(比如编译的临时文件)都不会被删除。如果需要一步到位的话可以尝试把两个命令配合起来使用。`git reset --hard` 之后再 `git clean -xdf`

#### 暂存
`git stash`能够让你保存当前的工作目录，相当于为你当前分支拍一个快照。保存快照的意义在于你不commit也能保留你做出的修改，之后你就可以切换到不同的分支上去。在其他分支上或者回到你stash过的分支上都可以把你拍过快照的修改应用在其之上，感觉十分方便。举个例子我遇到的使用场景就是，我在一个seo分支上一不小心写了一些快递功能的代码，而这些代码应该在delivery分支上才对。于是乎，我stash了seo分支，切回到master分支（为了进一步在delivery分支上开发），新建delivery分支，这时候delivery分支和master分支一模一样，这时候把之前的暂存应用在delivery分支上，这样我写的关于快递功能的代码就漂亮的转移到了delivery分支。这时候回到seo分支，把关于快递的修改撤销掉，再把快照删掉，干干净净！

`git stash`暂存当前工作目录

`git stash list`查看你所有的暂存

暂存恢复

一是用`git stash apply stash@{0}`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了






## 常见错误
Windows下使用`git add`时警告：warning：LF will be replaced by CRLF in ××××.××

出现原因：
这是因为在Windows中的换行符为CRLF，而在Linux中的换行符为LF。在git创建的项目中换行符为LF，而gits是linux环境，Git自作聪明的“换行符自动转换”功能会自动进行转换，然后系统会提示LF将被转换为CRLF。
解决的办法很简单，禁止git的自动转换即可。


#### 解决方法：
1、如果版本库/项目还没被创建，执行以下操作：
```bash
git config --global core.autocrlf false #在全局禁用自动转换
```
2、如果版本库/项目已经创建，使用非全局禁用自动转换
```bash
git config core.autocrlf false #在当前版本库中禁用自动转换
```
3、如果版本库/项目已经创建，在全局禁用自动转换则需要先删除之前创建的.git 文件后添加上面的设置。 
此操作很坑！是删除整个.git，会把跟远程的链接都断掉，整个git的提交历史/版本库都会被删除！
```bash
rm -rf .git
git config --global core.autocrlf false #在全局禁用自动转换
```
完成后再重新执行git创建版本库操作：
```bash
git init
git add ...
git remote add ***
```
这样设置git的配置后在执行add操作就没有问题了。



## 删除文件
```bash
rm -rf .git #删除.git文件夹
```
