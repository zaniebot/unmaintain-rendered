---
number: 12205
title: "missing wheel and `uv sync --upgrade --all-extras`"
type: issue
state: closed
author: jorgeperezg
labels:
  - question
assignees: []
created_at: 2025-03-16T21:43:42Z
updated_at: 2025-03-17T02:37:37Z
url: https://github.com/astral-sh/uv/issues/12205
synced_at: 2026-01-10T01:25:17Z
---

# missing wheel and `uv sync --upgrade --all-extras`

---

_Issue opened by @jorgeperezg on 2025-03-16 21:43_

### Summary

Today I was surprised by an error I got when trying to update a project.
uv sync --upgrade --all-extras

Resolved 151 packages in 695ms
error: Distribution `eccodes==2.40.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_39_x86_64`), but `eccodes` (v2.40.1) only has wheels for the following platforms: `macosx_13_0_arm64`, `macosx_13_0_x86_64`, `win_amd64`


In this project, cfrib is an extra and it has a dependency on eccodes but without pinning to any specific version. Everything was working fine with eccodes==2.40.0
I tried from scratch and got the same error with:
uv init
uv add cfgrib

however, uv add eccodes==2.40 works fine and uv add cfgrib afterwards works fine too. Even uv sync --upgrade works.

Is it reasonable to fail when unable to use that new version of eccodes or should uv simply use the latest working version?

### Platform

Ubuntu 24.04.2 LTS

### Version

uv 0.6.6

### Python version

Python 3.13.2

---

_Label `bug` added by @jorgeperezg on 2025-03-16 21:43_

---

_Comment by @charliermarsh on 2025-03-16 23:33_

There's a fairly extensive writeup of this problem here: https://docs.astral.sh/uv/concepts/resolution/#required-environments. But if you want to ensure that we always include a wheel for Linux, try:

```toml
[tool.uv]
required-environments = [
    "sys_platform == 'linux'"
]
```

---

_Label `bug` removed by @charliermarsh on 2025-03-16 23:34_

---

_Label `question` added by @charliermarsh on 2025-03-16 23:34_

---

_Renamed from "missing wheel and uv sync --upgrade --all-extras" to "missing wheel and `uv sync --upgrade --all-extras`" by @charliermarsh on 2025-03-16 23:34_

---

_Comment by @jorgeperezg on 2025-03-17 02:35_

Thanks for your reply. That's a perfect solution for my use case and the documentation explains it very well.

---

_Closed by @jorgeperezg on 2025-03-17 02:35_

---

_Comment by @charliermarsh on 2025-03-17 02:37_

Thanks for following up :)

---
