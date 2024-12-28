---
title: 修改 Git 代理设置出现的问题
date: 2021-03-07 09:11:47
tags: Git
categories:
    - Git
---
关于我因 git clone 慢而去给 git 设置代理而引发的那些事
<!-- more -->
## 使用到的命令
```
$ git config -l 查看所有配置
$ git config --global -l
```

## 过程
按着 [一招 git clone 加速](https://juejin.cn/post/6844903862961176583) 这篇文章设置了 git 的代理但是没用（应该是自己没设置正确），现在想想应该是当时没有 reset 仅代理 github 的配置，导致从 github 上下载项目时还是走错误的代理设置。
 __当时使用这两个命令重置__ 。
```
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
```

因为以上的问题，还影响到了 hexo 的一些操作，比如 deploy 和 init。

尝试一番发现（还卸载重装了 git）只是因为第一步使用 reset 只是重置了这两项，还有两条仅代理 github 的配置没有重置。
```
$ git config --global http.proxy http://127.0.0.1:1081
$ git config --global https.proxy https://127.0.0.1:1081
```
__使用这两条命令重置代理 github 的配置。__
```
$ git config --global --unset http.https://github.com.proxy
$ git config --global --unset https.https://github.com.proxy
```

## 总结
下次遇到这种问题应该先尝试恢复自己的操作，别一股脑的卸载重装