---
title: GITALK
categories:
  - Config
date: 2021-05-30 04:02:15
tags: 
  - Hexo
---
为小窝增加评论系统
<!-- more -->

# 部署 GITALK
1. 首先创建 [Github Application](https://github.com/settings/applications/new)

![](githubapp.jpg)

2. GITALK 配置
```yml
  enable: true  
  github_id: <Github 账号> # GitHub repo owner
  repo: <用来存放评论的仓库> # Repository name to store issues
  client_id: <上面注册的> # GitHub Application Client ID
  client_secret: <<上面注册的>> # GitHub Application Client Secret
  admin_user: <初始化仓库的账号>
  proxy: https://cors-anywhere.herokuapp.com/https://github.com/login/oauth/access_token # This is official proxy adress
```

3. Workers 配置
```
  whitelist = [ ".*" ]  # 所有请求可用
  whitelist = [ "^http.?://littlestate.github.io$", "littlestate.github.io$" ]
```

4. 最后一步
  将 `https://cors-anywhere.herokuapp.com` 替换为 Workers 中右边的地址

## 让标签和分类页不显示 GITALK 评论框
- 在 `hexo/source` 下 `tags`、`categories` **文件夹**内的 `index.md` 中添加 
```yml
  comments: false
```

# 参考
1. [使用 CloudFlare Workers 反向代理](https://www.chenhanpeng.com/create-own-cors-anywhere-to-resolve-the-request-with-403/)
2. [cors-anywhere 框架](https://github.com/Hanpeng-Chen/cloudflare-cors-anywhere/blob/master/index.js)