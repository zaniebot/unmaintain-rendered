---
number: 9105
title: "(🐞) `uv add` duplicates comments in pyproject.toml"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
assignees: []
created_at: 2024-11-13T23:39:30Z
updated_at: 2024-11-14T02:14:00Z
url: https://github.com/astral-sh/uv/issues/9105
synced_at: 2026-01-10T01:24:36Z
---

# (🐞) `uv add` duplicates comments in pyproject.toml

---

_Issue opened by @KotlinIsland on 2024-11-13 23:39_

```toml
[project]
name = "epic-based-project"
version = "1"
description = "an EPIC project that is based 😎"
requires-python = ">=3.13, <3.14"
dependencies = [
    # this is a comment
    "ruff>=0.7.3",
]
```
```
> uv add basedmypy  # we need basedmypy to be based
Resolved 6 packages in 0.01s
Prepared 1 package in 0.01s
Installed 1 package in 0.01s
 + basedmypy==2.7.0
```
```toml
[project]
name = "myhr"
version = "1"
description = "Automated testing for Queensland Health myHR"
requires-python = ">=3.13, <3.14"
dependencies = [
    # this is a comment
    "basedmypy>=2.7.0",
    # this is a comment
    "ruff>=0.7.3",
]
```

---

_Label `bug` added by @charliermarsh on 2024-11-14 00:05_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-14 01:26_

---

_Referenced in [astral-sh/uv#9109](../../astral-sh/uv/pulls/9109.md) on 2024-11-14 01:52_

---

_Closed by @charliermarsh on 2024-11-14 02:14_

---

_Closed by @charliermarsh on 2024-11-14 02:14_

---
