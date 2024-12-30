---
date: 2024-12-30T10:35:00+08:00
draft: false
title: Github Actions Self-Hosted 自动清理工作空间
---

# 使用 self-hosted

当使用自建的 runner 时，checkout 插件并没有自动清空工作空间，这时可以使用环境变量配置前置、后置脚本。

`ACTIONS_RUNNER_HOOK_JOB_STARTED=/opt/actions-runner/scripts/cleanup.sh`  
`ACTIONS_RUNNER_HOOK_JOB_COMPLETED=/opt/actions-runner/scripts/cleanup.sh`

```bash
#!/usr/bin/env bash
rm -rf $GITHUB_WORKSPACE/*
rm -rf $GITHUB_WORKSPACE/.??*
```

# 使用 self-hosted container

当使用 container 时就遇到问题了，我的 runner 使用单独的用户运行，并且没有 sudo 权限，而容器运行后，生成的工作空间全为 root 权限，导致我的后置任务，也无法删除。

这时可以使用容器来提权，修改工作空间的权限后再执行删除操作。

cleanup.sh 脚本: 
```bash
#!/usr/bin/env bash
# ================================================
# 文件路径 /opt/actions-runner/scripts/cleanup.sh
# ================================================

if [ -n "${GITHUB_WORKSPACE}" ]; then
  docker run --rm \
    -v $GITHUB_WORKSPACE/../../:/workspace \
    busybox:latest \
    /bin/sh -c "chown -R $(id -u):$(id -g) /workspace"

  rm -rf ${GITHUB_WORKSPACE}/{.??*,*}
fi
```

# 配置 .env

使用该清理脚本，需要在 `/opt/actions-runner/.env` 文件中添加环境变量，之后运行 `sudo ./svc.sh stop && sudo ./svc.sh start` 重启 runner
