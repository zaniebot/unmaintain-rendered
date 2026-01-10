---
number: 5255
title: "`uv init` creates unimportable package subdirectory"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - bug
  - good first issue
  - help wanted
  - preview
assignees: []
created_at: 2024-07-21T11:19:20Z
updated_at: 2024-07-22T16:09:26Z
url: https://github.com/astral-sh/uv/issues/5255
synced_at: 2026-01-10T01:23:47Z
---

# `uv init` creates unimportable package subdirectory

---

_Issue opened by @InSyncWithFoo on 2024-07-21 11:19_

Steps to reproduce:

* `mkdir a-b && cd a-b`
* `uv init`

This creates a package subdirectory with the same name as the project, but the project name is itself not a valid package name:

```shell
$ tree
.
├── README.md
├── pyproject.toml
└── src
    └── a-b
        └── __init__.py
```

Instead, the subdirectory should be named `a_b`, with `-` normalized.

---

_Comment by @charliermarsh on 2024-07-21 11:53_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-07-21 11:53_

---

_Label `preview` added by @charliermarsh on 2024-07-21 11:53_

---

_Comment by @charliermarsh on 2024-07-21 12:01_

We also need to clean the package name (in `pyproject.toml`), since the rules there are different from module names (e.g., can contain dashes): https://packaging.python.org/en/latest/specifications/name-normalization/#name-format

---

_Label `help wanted` added by @charliermarsh on 2024-07-21 12:19_

---

_Label `good first issue` added by @charliermarsh on 2024-07-21 17:38_

---

_Referenced in [astral-sh/uv#5292](../../astral-sh/uv/pulls/5292.md) on 2024-07-22 15:08_

---

_Closed by @konstin on 2024-07-22 16:09_

---

_Closed by @konstin on 2024-07-22 16:09_

---

_Referenced in [astral-sh/uv#12046](../../astral-sh/uv/issues/12046.md) on 2025-03-07 14:53_

---
