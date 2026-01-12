```yaml
number: 15478
title: make uv more easy use
type: issue
state: closed
author: GoFireStar
labels:
  - enhancement
assignees: []
created_at: 2025-08-24T07:23:16Z
updated_at: 2025-08-24T14:42:31Z
url: https://github.com/astral-sh/uv/issues/15478
synced_at: 2026-01-12T16:02:11Z
```

# make uv more easy use

---

_@GoFireStar_

### Summary

Refer to the following version management tools

the uv cmd is too long

nvm -v或 nvm --version | 查看 NVM 版本 | nvm -v
-- | -- | --
nvm install <version> | 安装指定版本的 Node.js | nvm install 18.12.1
nvm use <version> | 切换当前使用的 Node.js 版本 | nvm use 16.14.0
nvm ls或 nvm list | 列出本地已安装的版本（*标记当前版本） | nvm ls
nvm ls-remote | 列出远程可安装的所有版本 | nvm ls-remote
nvm uninstall <version> | 卸载指定版本的 Node.js | nvm uninstall 14.17.0
nvm alias default <version> | 设置默认使用的 Node.js 版本 | nvm alias default 20.5.1

jvms init | 初始化配置文件和目录结构（需管理员权限） | jvms.exe init
-- | -- | --
jvms list或 jvms ls | 列出已安装的 JDK 版本（*标记当前使用版本） | jvms list
jvms rls | 查看可在线安装的 JDK 版本列表 | jvms rls
jvms rls -a | 查看所有可用的 JDK 版本（包括历史版本） | jvms rls -a
jvms install <版本号> | 安装指定版本的 JDK（需完整版本号） | jvms install 17.0.6
jvms switch <版本号> | 切换当前使用的 JDK 版本 | jvms switch 11.0.15
jvms remove <版本号> | 卸载指定版本的 JDK | jvms remove 1.8.0_265
jvms proxy | 设置下载代理（如加速 GitHub 资源） | jvms proxy http://your-proxy:po

### Example

_No response_

---

_Label `enhancement` added by @GoFireStar on 2025-08-24 07:23_

---

_Comment by @zanieb on 2025-08-24 14:42_

I'm not entirely sure what you're concretely asking for here, but we won't be making broad changes to the uv interface to make it shorter. It's not just a version management tool.

---

_Closed by @zanieb on 2025-08-24 14:42_

---
