---
number: 11230
title: Not enough space on my Windows system, what should I do to move the UV content to the D drive?
type: issue
state: open
author: ghost
labels:
  - question
assignees: []
created_at: 2025-02-05T03:39:32Z
updated_at: 2025-03-08T11:35:29Z
url: https://github.com/astral-sh/uv/issues/11230
synced_at: 2026-01-10T01:25:02Z
---

# Not enough space on my Windows system, what should I do to move the UV content to the D drive?

---

_Issue opened by @ghost on 2025-02-05 03:39_

### Question

Include all caches, default download, dependency location

### Platform

windows 11

### Version

_No response_

---

_Label `question` added by @ghost on 2025-02-05 03:39_

---

_Comment by @ghost on 2025-02-05 03:59_

What command should I run to modify the environment variable, or in which file can I directly modify it?

---

_Comment by @zanieb on 2025-02-05 04:21_

You can use `uv cache prune` to clean up old cache entries.

The cache directory is https://docs.astral.sh/uv/configuration/environment/#uv_cache_dir

The Python install directory is https://docs.astral.sh/uv/configuration/environment/#uv_python_install_dir

---

_Comment by @ghost on 2025-02-05 08:14_

谢谢，按照这个做了，我想知道应该用什么命令修改环境变量来让我使用uv tool install 安装的依赖在D盘目录

---

_Comment by @HzFrancesca on 2025-03-08 11:34_

Windows GUI就设置里找“系统环境变量”

命令行就找如pwsh或者cmd写法，

如pwsh中`[Environment]::SetEnvironmentVariable("UV_PYTHON_INSTALL_DIR", "D:\UV\python", [EnvironmentVariableTarget]::User)`

随便问问Chatgpt就行

***
举个例子：

UV本体目录：UV_INSTALL_DIR

> scoop安的不用管

存放缓存：UV_CACHE_DIR

> D:\UV_storage\cache

存放工具本体：UV_TOOL_DIR

> D:\UV_storage\tools

存放二进制工具：UV_TOOL_BIN_DIR

> D:\UV_storage\bin

存放UV下载的Python：UV_PYTHON_INSTALL_DIR

> D:\UV_storage\python

存放二进制python：UV_PYTHON_BIN_DIR

> 指的是[这里](https://docs.astral.sh/uv/concepts/python-versions/#installing-python-executables)的二进制
>
>To install Python executables into your `PATH`, provide the `--preview` option:
>
> ```
> $ uv python install 3.12 --preview
> ```
>
> This will install a Python executable for the requested version into `~/.local/bin`, e.g., as `python3.12`.
>
>  change to:D:\UV_storage\bin

***

The follow commads  can be used for check.
```
uv cache dir
uv python dir
uv python dir --bin
uv tool dir
uv tool dir --bin
```

你应该指的是`uv tool dir` `uv tool dir --bin`这俩吧

---
