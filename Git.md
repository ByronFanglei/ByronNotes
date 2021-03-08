# Git命令学习
## 基础命令
### git配置
```shell
# 查看配置
git config -l
# 查看系统配置
git config --system --list
# 查看用户配置
git config --global --list
# 用户配置
git config --global user.name '用户名'
git config --global user.email '邮箱'
```

### git基本理论
* workspace：工作区，就是平时存放代码的地方
* stage：暂存区，用于临时存放你的改动，事实上只是一个文件，保存即将提交到文件列表信息
* repository：仓库区，就是安全存放数据的位置，这里面有你提交到所有版本的数据，其中HEAD指向最新放入仓库的版本
* remote：远程仓库

![](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7Ksu8UlITwMlbX3kMGtZ9p0NJ4L9OPI9ia1MmibpvDd6cSddBdvrlbdEtyEOrh4CKnWVibyfCHa3lzXw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7Ksu8UlITwMlbX3kMGtZ9p09iaOhl0dACfLrMwNbDzucGQ30s3HnsiaczfcR6dC9OehicuwibKuHjRlzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


```shell
# Git初始化：
git init
# 克隆远程仓库
git clone [url]
```

### git文件操作
* Untracked: 未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过git add 状态变为Staged.
* Unmodify: 文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改, 而变为Modified. 如果使用git rm移出版本库, 则成为Untracked文件
* Modified: 文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过git add可进入暂存staged状态, 使用git checkout 则丢弃修改过, 返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改 !
* Staged: 暂存状态. 执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为Unmodify状态. 执行git reset HEAD filename取消暂存, 文件状态为Modified

```shell
# 查看文件状态
git status

# 查看指定文件状态
git status [filename]

# 添加所有文件到暂存区
git add .
git add 文件名
git add 文件名1 文件名2 文件名3

# 提交暂存区的内容到本地仓库
git commit -m '注释'
# 提交暂存区的内容到本地仓库且跳过检测，比如eslint检测
git commit -m '注释' --no-verify

# 查看当前文件每一行都是由谁编码
git blame 文件名

# 暂存区为空，比较工作区和最后一次commit提交的仓库
git diff
# 比较暂存区和最后一次commit提交的仓库
git diff --cached
# 比较工作区和最后一次commit提交的仓库
git diff master

# 查看文件提交记录
git log
# 查看所有分支的所有操作记录
git reflog

# Git对文件大小写不区分，需要添加此命令
git config core.ignorecase false
```

* 忽略文件操作配置

```shell
# .gitignore文件
# 忽略所有.txt结尾的文件
*.txt
# byron.txt文件除外
!byron.txt
# 仅忽略项目根目录下的temp文件，不包括其他目录下的temp文件
/temp
# 忽略build/目录下的所有文件
build/
# 忽略doc/.txt文件，但不包括doc/server/a.txt
doc/*.txt
```


### git合并代码
#### git merge
* git merge 中合并两个分支时会产生一个特殊的提交记录，它有两个父节点

``` shell
# 查看分支
git branch
# 查看远程分支
git branch -r
# 查看所有分支
git branch -a
# 创建子分支
git branch children
# 创建子分支并切换到子分支
git checkout -b children
# 提交子分支
git checkout children
git commit -m '提交子分支'
# 提交主分支
git checkout master
git commit -m '提交主分支'
# 将子分支合并到主分支
git merge children

# 删除分支
git branch -d children
# 仅在合并后导致冲突时才使用，git merge --abort将会抛弃合并过程并且尝试重建合并前的状态
git merge --abort
```

#### git rebase
* Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去，Rebase 的优势就是可以创造更线性的提交历史

``` shell
# 创建子分支
git branch children
# 提交子分支
git checkout children
git commit 
# 提交主分支
git checkout master
git commit
# 再次切换到 children 分支，rebase 到 master 上
git rebase master
```

### git版本回退
```shell
# 查看问价历史版本
git log
git log --pretty=oneline

# 第一种回退操作
git reset --hard HEAD
git push origin master -f
# 回到过去后，要想再回到之前的最新版本的时候，则需要使用指令去查看历史操作得到最新的commit id

# 第二种回退操作
git revert HEAD
git push origin master

# 两种回退区别
reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。
revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在。
```


### HEAD
* HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录，HEAD 总是指向当前分支上最近一次提交记录

#### 分离HEAD
* 分离的 HEAD 就是让其指向了某个具体的提交记录而不是分支名

```shell
git checkout '分支名称'
```

#### 相对引用HEAD
```shell
# 切换到master的父节点上
git checkout master^
# 切换到master的第二个父节点上
git checkout master^^
```

```shell
~
```

### git远程仓库
```shell
# 克隆远程仓库
git clone 'ssh地址'
```

#### git fetch
* 从远程仓库下载本地仓库中缺失的提交记录
* 更新远程分支指针(如 o/master)
* 并不会改变你本地仓库的状态。它不会更新你的 master 分支，也不会修改你磁盘上的文件



### git stash
* 介绍：git stash 可用来暂存当前正在进行的工作， 比如想pull 最新代码， 又不想加新commit， 或者另外一种情况，为了fix 一个紧急的bug,  先stash, 使返回到自己上一个commit, 改完bug之后再stash pop, 继续原来的工作

```shell
# 工作流程
# 编写代码，但突然需要切换分支，需要修改bug，但又不想让当前写的代码commit，使用git stash设置缓存
git stash / git stash save "test-stash"
# 切换分支，修改bug后，返回之前的缓存
git stash pop # 恢复之前的缓存工作目录，删除栈第一个stash
git stash apply # 恢复之前的缓存工作目录
# 查看现有的stash（名字）
git stash list
# 移除stash
git stash drop 'stash名字'
# 删除第一个队列
git stash drop stash@{0}
git stash drop 0
# 移除全部stash
git stash clear
# 查看指定stash的diff
git stash show / git stash show -p # 查看全部diff
```

### 合并多个commit
* 当多人开发完毕后，每个人都会产生多个commit，为了便于后期的查看，需要将每个人的多次提交合并成一条提交

1. 压缩commit

```shell
# 压缩前6个commit
git rebase -i HEAD~6
# 压缩当前hash值的commit到指定hash值的commit
git rebase -i hash
# 执行后，进入vim，倒序排列，最下面的是最新的commit
```

2. 合并commit

```shell
# pick 的意识是要执行这个 commit
# squash 的意识是这个 commit 会被合并到前一个 commit
# 在vim中进行修改前缀，需要commit的为 pick(不变)，需要合并的修改为squash or s，修改完毕后 :wx or :x 退出vim
# 这里有一种情况，就是 3 4 5需要合并到1上，但是2需要commit，这时，就可以把第二条复制放到最后，然后，3 4 5改为 s，然后保存，退出vim即可
```

3. 是否有冲突

```shell
# git 会压缩提交历史，若有冲突，需要进行修改，修改的时候保留最新的历史记录，修改完之后输入以下命令
git add .
git rebase --continue

# 若想退出放弃此次压缩，执行以下命令
git rebase --abort

# 若没有冲突或者解决冲突后，进入commit message编辑页面，修改 commit message完毕后 :wx or :x 退出vim。
```

4. 同步到远程仓库

```shell
git push -f
# or
git push --force
# 执行完毕后，查看远程仓库commit是否合并到一起
```

### git命令速查表
![image-20210224151739968](https://img-blog.csdn.net/20180816164553616?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVxdWFucXVxbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)




