[TOC]

## 0 Git简介

使用场景：备份、代码还原、协同开发、问题追溯

常见版本控制方式

- **集中式**版本控制工具：如SVN，版本库存放在中央服务器，每个人从**中央服务器**下载代码，修改后提交回去
- **分布式**版本控制工具：如GIT，每个人电脑上都有完整版本库，工作无需联网，通过推送/拉取远程仓库代码进行代码协同/共享

## 1 Git工作流程

![git工作流程](.\resource\git工作流程.svg)

## 2 基本操作

### 2.1 初始化仓库

```shell
# 在当前文件夹下初始化git仓库
git init
```

### 2.2 文件管理

未暂存在idea里是绿色文件，未跟踪是蓝色文件

<img src=".\resources\git本地仓库操作.png" alt="git工作流程" style="zoom:50%;" />

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

## 5 注意事项

**切换分支之前要提交完当前分支记录，否则容易丢失代码**