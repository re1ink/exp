- [0 Git简介](#0-git简介)
- [1 Git工作流程](#1-git工作流程)
- [2 基本操作](#2-基本操作)
  - [2.1 初始化仓库](#21-初始化仓库)
  - [2.2 文件管理](#22-文件管理)
  - [2.3 版本回退](#23-版本回退)
  - [2.4 添加文件到忽略列表](#24-添加文件到忽略列表)
- [3 Git分支](#3-git分支)
  - [3.1 基本操作](#31-基本操作)
  - [3.2 分支合并](#32-分支合并)
  - [3.3 解决冲突](#33-解决冲突)
  - [3.4 分支命名原则](#34-分支命名原则)
- [4 远程仓库](#4-远程仓库)
  - [4.1 生成ssh公钥](#41-生成ssh公钥)
  - [4.2 绑定远程仓库](#42-绑定远程仓库)
  - [4.3 推送到远程仓库](#43-推送到远程仓库)
  - [4.4 克隆远程仓库](#44-克隆远程仓库)
  - [4.5 抓取/拉取远程分支](#45-抓取拉取远程分支)
  - [4.6 合并远程冲突](#46-合并远程冲突)
- [5 进阶操作](#5-进阶操作)
  - [5.1 rebase](#51-rebase)
    - [5.1.1 对比merge和rebase](#511-对比merge和rebase)
    - [5.1.2 使用场景](#512-使用场景)
  - [5.2 revert](#52-revert)
    - [5.2.1 对比reset和revert](#521-对比reset和revert)
    - [5.2.2 使用场景](#522-使用场景)
  - [5.3 cherry-pick](#53-cherry-pick)
    - [5.3.1 基本操作](#531-基本操作)
    - [5.3.2 使用场景](#532-使用场景)
    - [5.3.3 冲突解决](#533-冲突解决)
- [6 注意事项](#6-注意事项)


## 0 Git简介

使用场景：备份、代码还原、协同开发、问题追溯

常见版本控制方式

- **集中式**版本控制工具：如SVN，版本库存放在中央服务器，每个人从**中央服务器**下载代码，修改后提交回去
- **分布式**版本控制工具：如GIT，每个人电脑上都有完整版本库，工作无需联网，通过推送/拉取远程仓库代码进行代码协同/共享

## 1 Git工作流程

<img src=".\resources\git工作流程.svg" alt="git工作流程" />

## 2 基本操作

### 2.1 初始化仓库

```shell
# 在当前文件夹下初始化git仓库
git init
```

### 2.2 文件管理

未暂存在idea里是绿色文件，未跟踪是蓝色文件

<img src=".\resources\git本地仓库操作.png" alt="git本地仓库操作" style="zoom:50%;" />

```shell
# 添加所有文件到暂存区
git add .
# 添加指定文件到暂存区
git add test.txt
# 将文件从暂存区恢复到工作区
git restore --staged test.txt
# 提交暂存区文件到本地仓库，-m后写入提交信息
git commit -m "add file"
# 查看提交记录
git log
# 查看提交记录，指定优化
git log --pretty=oneline --abbrev-commit --all --graph
```

### 2.3 版本回退

```shell
# 查看提交记录
# a8584@Admin MINGW64 /d/Program Files/GitTest (master)
# $ git-log
# * 68bd320 (HEAD -> master) modify file
# * 0006fd5 add test.txt
git log

# 回退提交 git reset --hard ${commitID}
git reset --hard 0006fd5
```

注意：`git reset --hard`不代表直接把代码删除了，可以通过同样的命令将提交重置到任意指定提交处

```shell
# 查看之前的所有操作，包括reset命令
# $ git reflog
# 0006fd5 (HEAD -> master) HEAD@{0}: reset: moving to 0006fd5
# 68bd320 HEAD@{1}: commit: modify file
# 0006fd5 (HEAD -> master) HEAD@{2}: commit (initial): add test.txt
git reflog

# 回到修改分支
git reset --hard 68bd320
```

### 2.4 添加文件到忽略列表

```shell
# 创建git文件忽略列表
touch .gitignore
# 忽略所有.a结尾的文件
*.a 
# 忽略build文件夹下的所有文件
build/
# 忽略doc文件夹当前层所有txt文件
doc/*.txt
# 忽略doc文件夹下所有层的txt文件
doc/**/*.txt
```

## 3 Git分支

### 3.1 基本操作

```shell
# 查看当前所有分支
git branch
# 新建分支
git branch new_branch
# 切换分支
git checkout new_branch
# 切换并创建分支
git checkout -b dev_01
# $ git-log
# * 854adc9 (master) add ignore file
# * 68bd320 (HEAD -> new_branch) modify file
# * 0006fd5 add test.txt
# HEAD指向当前分支，可以发现new_branch比master少了一份提交
```

### 3.2 分支合并

``` shell
# 合并new_branch的修改到当前分支
git merge new_branch 
```

### 3.3 解决冲突

找到冲突的部分并手动解决

```shell
# 打开冲突文件，看到内容
# $ cat test.txt
# hello world
# modify
# <<<<<<< HEAD    当前分支的修改
# master edit     具体的修改内容
# =======
# add dev02       具体的修改内容
# >>>>>>> dev02   dev02分支的修改

# 修改完，同之前流程
git add .
git commit
```

### 3.4 分支命名原则

不同项目命名不同，具体情况具体分析

## 4 远程仓库

### 4.1 生成ssh公钥

```shell
# 生成ssh公钥
ssh-keygen -t rsa
# 查看公钥位置，复制公钥到github设置
cat ~/.ssh/id_rsa.pub
# 认证
ssh -T git@github.com
```

### 4.2 绑定远程仓库

```shell
# 绑定远程仓库
git remote add origin git@github.com:re1ink/test.git
# 查看当前绑定的远程仓库
git remote
```

### 4.3 推送到远程仓库

```shell
# 推送主分支到仓库
git push origin master
```

### 4.4 克隆远程仓库

```shell
# 克隆远程仓库
git clone git@github.com:re1ink/test.git
```

### 4.5 抓取/拉取远程分支

```shell
# 抓取远程分支
# fetch: 只抓取远程修改，但不合并
git fetch
# 合并远程修改
git merge origin/master
# 拉取远程分支，二合一
git pull
```

### 4.6 合并远程冲突

`git pull`，修改冲突后，重新`push`上去

## 5 进阶操作

### 5.1 rebase

#### 5.1.1 对比merge和rebase

**merge**

- 两个旧提交会合并成一个新提交
- 保留原有分支的结构、顺序
- 如果使用大量merge，分支结构会很乱

<img src=".\resources\git merge.svg" alt="git merge" />

**rebase**

- 会打乱原有的提交结构和时间顺序
- 操作：找到两个分支的公共祖先，把所有提交桥接在被合入分支的后面
- rebase过程中被桥接的节点会创建新的提交(commitID不同)
- 比merge操作更加简洁

<img src=".\resources\git rebase.svg" alt="git rebase" />

#### 5.1.2 使用场景

- `rebase`: 相同分支需要合并内容时，比如`v1-dev`和`origin/v1-dev`合并时
- `merge`: 不同分支合并内容时，比如`v1-dev-test`和`origin/v1-dev`合并时

### 5.2 revert

#### 5.2.1 对比reset和revert

**reset**

reset重置本地当前分支的HEAD指针，有三种方式，假定当前提交为A→B→C→D，HEAD指向D

- --hard B: 直接重置HEAD指针到B，C和D修改丢失，只能通过git reflog还原
- --mix B: 重置HEAD指针到B，C和D修改回到**工作区**
- --soft B: 重置HEAD指针到B，C和D修改回到**暂存区**

> 注意: git reset 的操作历史仅记录于本地 reflog，不会随 git push 同步到远程。因此，协同方通过 git pull 获取的是重置后的结果状态，而非你的操作过程。这会导致双方仓库历史出现分叉，协同方必须通过 git reset 等命令在本地强制对齐分支，从而显著增加代码协同的复杂度和风险

**revert**

revert不重置HEAD指针，而是创建一个和撤掉提交完全相反的提交，抵消错误提交带来的影响，假定当前提交为A→B→C→D，此时使用`git revert B`，提交变为A→B→C→D→B'，B'和B抵消，实际的提交效果为A→C→D

> 注意: 由于git revert是新增一个提交，因此这个提交会同步到远程，此时协同方拉取代码后会把revert记录一同拉取，而无需进行额外对齐操作

#### 5.2.2 使用场景

- `reset`: 本地已commit但未push的代码，需要重构或重新组织提交时
- `revert`: 已经push的代码，需要回退修改时

### 5.3 cherry-pick

摘樱桃，顾名思义，它的功能是对已经存在的commit进行挑选，选择自己想要的commit提交到指定分支

#### 5.3.1 基本操作

```shell
# 1. 摘取单个提交
git cherry-pick <commit-hash>

# 2. 摘取多个连续或不连续的提交
git cherry-pick <hash1> <hash3> <hash5>

# 3. 摘取一个区间的提交（左开右闭，不含start-commit，含end-commit）
git cherry-pick <start-commit>..<end-commit>

# 4. 摘取一个区间的提交（左闭右闭，含start-commit，含end-commit）
git cherry-pick <start-commit>^..<end-commit>

# 5. 摘取时不在当前分支立即提交，只保留工作区更改(将所有pick的修改应用到暂存区和工作区，此时可以对pick结果进一步修改)
git cherry-pick -n <commit-hash>
# 或 --no-commit

# 6. 遇到冲突后，继续执行cherry-pick(要解决完当前commit的冲突才能continue，否则会报错)
git cherry-pick --continue

# 7. 遇到冲突后，放弃cherry-pick，恢复原状(放弃之前处理的所有pick，回到初始状态)
git cherry-pick --abort
```

#### 5.3.2 使用场景

- **紧急修复bug**: dev分支修复bug，此时release分支需要马上上线这个修复，pick对应bug修改的commit进行紧急修复
- **分支提交错误**: 把文件提交给了错误的分支，假设要提交的是分支dev，实际提交的是master
  - dev：通过cherry-pick把对main的提交摘取过来
  - master：根据commit是否已经push来判断使用reset还是revert
- **功能同步**: 有一个模块在项目A使用，此时项目B要同步A的这个模块，使用cherry-pick同步这个模块的所有修改

#### 5.3.3 冲突解决

如果同时pick多个文件，需要一个个文件解决冲突、add、commit，之后使用`--continue`继续下一个pick，周而复始，直到所有pick的冲突处理完毕

## 6 注意事项

- 切换分支之前要提交完当前分支记录，否则容易丢失代码
- 不同分支合并时不要使用rebase