```yaml
number: 9076
title: "(🐞) `uv add` alphabetic preservation sorts prefixes after longer entries"
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-11-13T04:04:51Z
updated_at: 2024-11-13T20:47:58Z
url: https://github.com/astral-sh/uv/issues/9076
synced_at: 2026-01-12T15:59:41Z
```

# (🐞) `uv add` alphabetic preservation sorts prefixes after longer entries

---

_@KotlinIsland_

i would think that `pylint` would come before `pylint-module-boundaries`

```py
[tool.uv]
dev-dependencies = [
  # should be in alphabetical order
  "pylint>=3.2.6",
  "pylint-module-boundaries>=1.3.1",
  "ruff>=0.6.2,<7",
]
```
```
> uv add robotcode[analyze]==0.97.0 --dev
```
```py
[tool.uv]
dev-dependencies = [
  # should be in alphabetical order
  "pylint>=3.2.6",
  "pylint-module-boundaries>=1.3.1",
  "ruff>=0.6.2,<7",
  "robotcode[analyze]==0.97.0",
]
```

# ------------------------
```py
[tool.uv]
dev-dependencies = [
  # should be in alphabetical order
  "pylint-module-boundaries>=1.3.1",
  "pylint>=3.2.6",
  "ruff>=0.6.2,<7",
]
```
```
> uv add robotcode[analyze]==0.97.0 --dev
```
```py
[tool.uv]
dev-dependencies = [
  # should be in alphabetical order
  "pylint-module-boundaries>=1.3.1",
  "pylint>=3.2.6",
  "robotcode[analyze]==0.97.0",
  "ruff>=0.6.2,<7",
]
```

---

_Renamed from "(🐞) `uv add` alphabetic preservation sorts prefixes after loger entries" to "(🐞) `uv add` alphabetic preservation sorts prefixes after longer entries" by @KotlinIsland on 2024-11-13 04:06_

---

_Comment by @zanieb on 2024-11-13 04:07_

I would think so too!

---

_Label `bug` added by @zanieb on 2024-11-13 04:07_

---

_Label `good first issue` added by @zanieb on 2024-11-13 04:07_

---

_Comment by @zanieb on 2024-11-13 04:07_

(I'm not sure this is actually easy to fix, but hopefully it is!)

---

_Comment by @charliermarsh on 2024-11-13 16:34_

I think the issue is we (erroneously) compare the entire string including the specifiers -- so it's not `pylint` vs `pylint-module-boundaries`, it's `pylint>=3.2.6` vs. `pylint-module-boundaries>=1.3.1`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-13 16:37_

---

_Closed by @charliermarsh on 2024-11-13 20:47_

---
