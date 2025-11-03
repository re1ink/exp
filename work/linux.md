- [1 linux常用命令](#1-linux常用命令)
  - [1.1 生成ssh公钥](#11-生成ssh公钥)
  - [1.2 指定操作别名](#12-指定操作别名)

# 1 linux常用命令

## 1.1 生成ssh公钥

```shell
# 生成ssh公钥
ssh-keygen -t rsa
# 查看公钥位置，复制公钥到github设置
cat ~/.ssh/id_rsa.pub
# ssh -T git@github.com
```

## 1.2 指定操作别名

```shell
# 创建.bashrc
touch .bashrc

# .bashrc文件中配置命令缩写
alias k='kubectl'
```