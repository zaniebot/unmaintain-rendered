---
number: 16525
title: Can python versions be restricted on a specific sys_platform?
type: issue
state: closed
author: mhicks-cfs
labels:
  - question
assignees: []
created_at: 2025-10-30T21:53:31Z
updated_at: 2025-11-01T20:26:04Z
url: https://github.com/astral-sh/uv/issues/16525
synced_at: 2026-01-07T13:12:19-06:00
---

# Can python versions be restricted on a specific sys_platform?

---

_Issue opened by @mhicks-cfs on 2025-10-30 21:53_

### Question

I am currently trying to update a `uv` project to add py3.13 support to the project. Unfortunately there is a issue with dependencies that is currently preventing this on MacOS (Darwin). We would like to allow Windows and Linux to upgrade, but hold back Darwin until the upstream dependency issue is resolved.

Is it possible to have different python version ranges for the Darwin `sys_platform` than for other `sys_platform`s? Specifically we need 3.11-3.12 on Darwin and 3.11-3.13 for all others. I could not find this described in the docs, and a error was emitted when I tried to use the same syntax for specifying different package dependencies per platform for this.

### Platform

Linux, Windows, Darwin

### Version

0.9.6

---

_Label `question` added by @mhicks-cfs on 2025-10-30 21:53_

---

_Comment by @muhammad-fiaz on 2025-10-31 06:31_

@mhicks-cfs you can do that by `markers`  and also using uv prebuild [settings](https://docs.astral.sh/uv/reference/settings/)

also there's feature for [Limited resolution environments](https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments) 

also you can use like this
```toml
[project]
dependencies = ["httpx"]

[tool.uv.sources]
httpx = [
  { git = "https://github.com/encode/httpx", tag = "0.27.2", marker = "sys_platform == 'darwin' and python_version >= '3.11'" },
  { git = "https://github.com/encode/httpx", tag = "0.24.1", marker = "sys_platform == 'linux' and python_version < '3.11'" },
]
```
or 
```toml
[project]
dependencies = ["torch"]

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "platform_system == 'Darwin'"},
  { index = "torch-gpu", marker = "platform_system == 'Linux'"},
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-gpu"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```
for above refer [here](https://docs.astral.sh/uv/concepts/projects/dependencies/#multiple-sources)

i hope this helps :)
 

---

_Comment by @mhicks-cfs on 2025-10-31 22:39_

Thanks @muhammad-fiaz, using Limited Resolution Environments ended up working. If anyone comes across the same question here is a solution to limit python on MacOS but not other platforms. With this configuration the lock file does not contain a resolution for 3.13 under MaxOS, as desired:
```
[project]
requires-python = ">=3.11, <3.14"
...

[tool.uv]
environments = [
    "sys_platform == 'darwin' and python_version < '3.13'",
    "sys_platform != 'darwin'",
]
```

I do think this may be a good example to add to the docs as well since it is non-obvious that it differs from the way package dependencies are specified for different platforms.

---

_Comment by @charliermarsh on 2025-11-01 20:26_

Yeah, that solution looks reasonable to me (and I think it should also fail (correctly) if you try to install on macOS with Python 3.14).

---

_Closed by @charliermarsh on 2025-11-01 20:26_

---
