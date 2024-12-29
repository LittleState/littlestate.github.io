---
title: Hexo 搭建日记
date: 2021-03-06 15:05:18
tags: 
  - Hexo
categories: 
  - Config
---
记录一下第一次搭建 Hexo 的各种问题
<!-- more -->
## 教程
1. [Hexo 官方教程](https://hexo.io/zh-cn/)
2. [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
3. [超详细 Hexo+Github 博客搭建小白教程](https://zhuanlan.zhihu.com/p/35668237)
4. [多级 categories](https://aiellochan.com/2018/02/13/hexo/Hexo-%E4%B8%80%E7%AF%87%E6%96%87%E7%AB%A0%E5%A4%9A%E4%B8%AA-categories/)

### hexo 基本操作：
```bash
$ hexo g #生成静态文件
$ hexo new [layout] <title> #layout 有post、draft、page
$ hexo s #启动本地 Server
$ hexo deploy #可简写为d
```

## 配置文件
### 站点配置文件：
```yml
$ line_number: false    #不显示代码块序号
$ post_asset_folder: true   #新建 post 文章时会顺带创建一个文件夹，用来存素材
```
### 主题配置文件：
```yml
$ scheme: Gemini    #主题布局
$ busuanzi_count: enable: true    #开启访问统计
$ copy_button: enable: true #开启代码块一键复制
$ creative_commons: #增加版权信息
```

## 主题
1. [主题美化](https://blog.mrzorg.top/Hexo/2020-02-12-hero-next-theme-settings/)
2. [设置点击爱心](https://forestneo.com/2018/12/01/%E7%B3%BB%E7%BB%9F%E9%85%8D%E7%BD%AE-Hexo-NexT%E4%B8%BB%E9%A2%98%E9%BC%A0%E6%A0%87%E7%82%B9%E5%87%BB%E7%88%B1%E5%BF%83%E7%89%B9%E6%95%88/)
3. [NexT (目前)](https://theme-next.iissnan.com/)
4. [Volantis](https://volantis.js.org/)
5. [Hugo主题-Mogeko](https://github.com/Mogeko/mogege)

## 遇到的一些问题
[Hexo next 主题配置右侧栏的分类和标签打开的是空白](https://www.cnblogs.com/lfri/p/12221359.html)