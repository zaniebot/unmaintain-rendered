---
number: 10339
title: "[Feature Request] uv add but not include version in pyproject.toml"
type: issue
state: closed
author: ayclqt
labels:
  - question
assignees: []
created_at: 2025-01-07T04:30:05Z
updated_at: 2025-01-07T18:37:03Z
url: https://github.com/astral-sh/uv/issues/10339
synced_at: 2026-01-07T13:12:18-06:00
---

# [Feature Request] uv add but not include version in pyproject.toml

---

_Issue opened by @ayclqt on 2025-01-07 04:30_

Can you add argument where uv add can install the latest version of package and add the pakage into pyproject.toml without version
For example:
uv add lxml
```pyproject.toml
[project]
dependencies = [
    "lxml>=5.3.0",
]
```
i want it not to have the version like this
```pyproject.toml
[project]
dependencies = [
    "lxml",
]
```
so whenever i create a new venv and uv sync it will always install the latest version

---

_Comment by @konstin on 2025-01-07 10:16_

The version is only a lower bound, uv will always try to select the latest version independent on whether there is a lower bound or not. The lower bound avoids bad cases with the resolver where we would try ancient versions to resolve a conflict between two packages.

---

_Label `question` added by @konstin on 2025-01-07 10:16_

---

_Comment by @zanieb on 2025-01-07 18:37_

You can use `--raw-sources` to achieve this.

---

_Closed by @zanieb on 2025-01-07 18:37_

---
