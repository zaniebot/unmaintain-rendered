---
number: 14019
title: "How to set folder for `tool.uv.sources`?"
type: issue
state: closed
author: hongbo-miao
labels:
  - question
assignees: []
created_at: 2025-06-13T11:42:03Z
updated_at: 2025-06-14T07:42:03Z
url: https://github.com/astral-sh/uv/issues/14019
synced_at: 2026-01-07T13:12:18-06:00
---

# How to set folder for `tool.uv.sources`?

---

_Issue opened by @hongbo-miao on 2025-06-13 11:42_

### Question

```toml
[project]
name = "my-project"
version = "1.0.0"
requires-python = "~=3.12.0"
dependencies = [
  "sglang[all]",
]

[tool.uv]
package = false
required-version = ">=0.6.0"

[tool.uv.sources]
sglang = { git = "https://github.com/sgl-project/sglang.git", branch = "main" }
```

This failed with

```sh
  × Failed to download and build `sglang @ git+https://github.com/sgl-project/sglang.git@main`
  ╰─▶ /home/hongbo-miao/.cache/uv/git-v0/checkouts/b454a06192129efd/8ab7d93c does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
````

Because `sglang` is a mono repo, I need go to folder [sgl-kernel](https://github.com/sgl-project/sglang/tree/main/sgl-kernel)

How to set folder for `tool.uv.sources`? Thank you!

### Platform

Linux 6.11.0-26-generic x86_64 GNU/Linux

### Version

uv 0.7.8 (0ddcc1905 2025-05-23)

---

_Label `question` added by @hongbo-miao on 2025-06-13 11:42_

---

_Comment by @konstin on 2025-06-13 12:39_

With `subdirectory`, as described in https://docs.astral.sh/uv/concepts/projects/dependencies/#git.

---

_Comment by @hongbo-miao on 2025-06-14 05:52_

Thank you so much @konstin ! Sorry, I missed it before.

---

_Closed by @hongbo-miao on 2025-06-14 07:42_

---
