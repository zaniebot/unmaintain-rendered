---
number: 16577
title: git dependency HTTPS/SSH differentiation based on platform markers
type: issue
state: open
author: vasile-c
labels:
  - question
assignees: []
created_at: 2025-11-03T10:59:51Z
updated_at: 2025-11-05T18:09:14Z
url: https://github.com/astral-sh/uv/issues/16577
synced_at: 2026-01-07T13:12:19-06:00
---

# git dependency HTTPS/SSH differentiation based on platform markers

---

_Issue opened by @vasile-c on 2025-11-03 10:59_

### Summary

Hello,

When trying to differentiate the protocol to use for a git dependency based on the platform:
```pyproject
[tool.uv.sources]
private_dep = [
    { git = "ssh://git@github.com/private_orga/private_dep.git", marker = "sys_platform != 'win32'" },
    { git = "https://github.com/private_orga/private_dep.git", marker = "sys_platform == 'win32'" },
]
```

`uv sync` still tries to access the dependency using **both** protocols on any platform:
```shell
> uv sync
  Updated ssh://git@github.com/private_orga/private_dep.git
  Updated https://github.com/private_orga/private_dep.git
```

And if one of the platforms doesn't support both protocols:
```shell
> uv sync
  Updating ssh://git@github.com/private_orga/private_dep (HEAD)
  Updated https://github.com/private_orga/private_dep.git
  ⠹ Resolving dependencies...
  × Failed to download and build `private_dep @ git+ssh://git@github.com/private_orga/private_dep.git`
  ├─▶ Git operation failed
  [...]
```

Should `uv` be able to install git dependencies using different protocols on different platforms?

Thank you for all your work at Astral!

### Platform

macOS 15 ARM64, Windows 11 AMD64

### Version

uv 0.9.7

### Python version

_No response_

---

_Label `bug` added by @vasile-c on 2025-11-03 10:59_

---

_Comment by @konstin on 2025-11-05 18:09_

uv needs to know the metadata of both URLs for both platforms to build the lockfile, so it tries to fetch both, which creates the error you see. The marker in sources isn't well suited for this case, we recommend instead using the same protocol consistently across platforms.

---

_Label `bug` removed by @konstin on 2025-11-05 18:09_

---

_Label `question` added by @konstin on 2025-11-05 18:09_

---
