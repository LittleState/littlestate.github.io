---
title: 初识 Git
date: 2021-03-12 20:39:16
tags: Git
categories:
    - Git
---
第一次使用Git
<!--more-->
## git 命令详解
[Git 命令手册](https://gitee.com/opensource-guide/git_tutorial/Git%20%E5%91%BD%E4%BB%A4%E8%AF%A6%E8%A7%A3/%E5%B8%B8%E7%94%A8%20Git%20%E5%91%BD%E4%BB%A4/#%E9%85%8D%E7%BD%AE%E7%9B%B8%E5%85%B3)
## 用户信息
- 省略 --global 参数代表仅本项目生效
```bash
$ git config --global user.name "<你的名字>"
$ git config --global user.email <你的邮箱>
```
## init 初始化本地仓库
`git init`
## clone 获取远程仓库
```bash
$ git clone https://gitee.com/giteeeer/example.git   #这样仅仅是将默认分支同步到本地
```
## remote 获取仓库的地址
```bash
$ git remote -v     #fetch是获取的地址；push 是提交的地址
```
## branch 获取分支列表
```bash
$ git branch
```
## add 让 git 跟踪变更
```bash
$ git add test.txt  #比如这样就是让git可以“看到”新建的txt文件；会把它放入缓冲区实现跟踪
$ git status    #获得文件状态
```
- Changes to be committed: 新加入缓冲区的文件
- Changes not staged for commit: 已在缓冲区中被修改了的文件
- Untracked files: 未加入缓冲区的文件
## .gitignore
- 可以编辑该文件让 git 忽略文件
## pull
- 访问远程仓库，将本地没有的数据全部拉取到本地
- 会尝试合并到当前分支
## fetch
- 和 pull 不同的是不会自动帮你合并，而是要自己手动合并
## diff 对比不同
- 通过使用 diff 命令可以让我们详细对比每个被追踪的文件的变更
## commit 提交到本地存储库
```bash
$ git commit -m "修改介绍"
```
- git 会返回一个报告，会显示修改、添加、删除了什么；提交到了哪个分支，还有SHA-1校验和
- 还可以添加 `-a` 参数，相当于直接 `add + commit` 了
- `amend` 如果这次提交忘记了一些文件，可以使用这个参数补传
    - 使用 log 只能看到一次提交记录，第二次的 `amend` 会覆盖第一次提交
## rm
- 只使用 rm 在本地删除没有用，还需要让 git 记录这次操作使用 `git rm`
- 如果要删除的文件已经放入暂存区了，就需要加上 `-f` 参数强制删除
- 如果只是希望从暂存区移除文件，则使用 `--cache` 而不会删除本地文件
## mv 移动/重命名
- `git mv` 相当于先 rm 暂存区的文件，再 add 改名后的文件
- `git mv` 只能操作已经被 git 追踪的文件
## reset HEAD 取消暂存的文件
- 当我们希望取消某一个文件的暂存的时候，以“toUntracked.txt”为例，是我们刚刚加入暂存区的文件
```bash
$ git reset HEAD toUntracked.txt
```
## checkout 还原
```bash
$ git checkout -- project.txt
```
- 会用这个文件提交过的版本覆盖本地版本
## log
- 查看提交历史
## push
```bash
$ git push
```