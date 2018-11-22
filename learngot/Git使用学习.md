



# <center>Git使用学习</center>

## 本地仓库初始化

创建本地仓库地址

`$ git init`

## 提交文件

`$ git add test.txt`

`$ git commit -m "修改备注"`

## 版本控制

### 版本回退

查看文件状态

`$ git status`

查看修改内容

`$ git diff <文件名>`

查看修改记录

`$ git log`

`$ git log --pretty = oneline`--记录在一行显示出来

版本退回

`$ git reset --hard HEAD^`--退回上一个版本

`$ git reset --hard HEAD~n`--退回上n个版本

`$ git reset --hard <commit id>`--退回指定提交ID的版本（提交ID可为其中4至5位）

查看历史命令

`$ git reflog`

### 工作区与暂存区



![typora.jpg](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)



stage为暂存区，`git add`命令实际上就是把要提交的所有修改放到暂存区，然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的

```
$ git status
On branch master
nothing to commit, working tree clean
```

### 管理修改

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> ...第n次修改 -> `git add` -> `git commit`

多次修改添加暂存区后一并提交

### 撤销修改

把文件在工作区的修改全部撤销，有以下两种情况：

```
$ git checkout -- <文件名>
```

1. 文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2. 文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。



把已放到暂存区的文件修改撤销掉，重新放回工作区

`
$ git reset HEAD <文件名>`

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

### 删除文件

从本地文件库删除文件

`$ rm <文件名> `

查找哪些文件被删除了

`$ git status`

```
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    deleted:    <文件名>

no changes added to commit (use "git add" and/or "git commit -a")
```

分成以下两种情况：

1. 确实需要删除，需要再从版本库中删除

   `$ git rm <文件名>`

   `$ git commit -m "<文件名>"`

2. 删除错误，将误删文件从版本库恢复，但只能恢复文件到最新版本，会丢失**最近一次提交后你修改的内容**

   `$ git checkout -- <文件名>`

## 远程仓库

创建远程仓库公钥

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

### 添加远程库

将本地git库与GitHub远程库origin关联

`$ git remote add origin git@github.com:<Github账户名>/<Github-Repository>`

第一次推送master分支的所有内容

`$ git push -u origin master`

解除与GitHub远程库origin关联

`$ git remote rm origin`

### 克隆远程库

在GitHub建立远程库

将GitHub远程库克隆至本地gitku

`$ git clone git@github.com:<Github账户名>/<Github-Repository>`

## 创建分支

### 创建与合并分支

#### 创建分支流程

##### 1.正常添加，head指针指向master分支

![typora.jpg](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

##### 2.创建分支指针dev，再将head指针指向dev，工作区文件不变

![typora.jpg](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

##### 3.最新一次工作区提交只针对dev分支，而master指针不变

![typora.jpg](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

##### 4.在dev指针上完成工作后，把dev合并到master上

![typora.jpg](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)

##### 5.合并后可以选择删除dev分支指针

![typora.jpg](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)

##### 常用命令

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

#### 冲突解决

分支与master合并出现冲突时需要手工进行冲突解决后再合并

使用命令查看各分支修改内容

`$ cat <文件名>`

使用带参数的命令查看分支合并情况

`$ git log --graph --pretty = oneline --abbrev `

#### 合并策略

合并分支时增加参数避免直接合并提交

`$ git merge --no-ff -m "merge with no-ff" <分支名>`

#### 分支暂存

有bug要在master紧急修复，但工作分支上未提交，此时需要暂存工作分支

切换至工作分支

`$ git checkout <分支名>`

暂存工作分支内容

`$ git stash`

master创建临时分支issue-001

```
$ git checkout master
$ git checkout -b issue-001
```

修复完成后合并至master

```
$ git checkout master
$ git merge --no-ff -m "合并备注" issue-001
```

切回工作分支查看暂存

```
$ git checkout <分支名>
$ git stash list
```

回复工作分支现场

1. stash内容不删除

```
$ git stash apply
$ git stash drop
```



2. stash内容直接删除

```
$ git stash pop
```

多次stash后恢复指定stash内容

```
$ git stash list
$ git stash apply stash@{stash编号}
```

#### 分支删除

不合并分支下删除分支，需要强制删除并丢失修改

```
$ git branch -D <分支名>
```

#### 多人协作

查看远程库信息

`$ git remote -v`

推送分支

`$ git push orgin <分支名>`

