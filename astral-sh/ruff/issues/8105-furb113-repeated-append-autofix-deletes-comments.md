```yaml
number: 8105
title: "`FURB113` (repeated-append) autofix deletes comments"
type: issue
state: closed
author: tdulcet
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-10-21T14:41:02Z
updated_at: 2023-11-09T03:10:12Z
url: https://github.com/astral-sh/ruff/issues/8105
synced_at: 2026-01-12T15:54:47Z
```

# `FURB113` (repeated-append) autofix deletes comments

---

_@tdulcet_

* A minimal code snippet that reproduces the bug.
```py
nums = [1, 2, 3]

nums.append(4)
# Critically important comment 1
nums.append(5)
# Critically important comment 2
nums.append(6)
```
Result:
```py
nums = [1, 2, 3]

nums.extend((4, 5, 6))
```
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```
ruff --select FURB113 --preview --isolated --unsafe-fixes --fix test.py
```
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
None
* The current Ruff version (`ruff --version`).
```
ruff 0.1.1
```

---

_Label `bug` added by @zanieb on 2023-10-21 16:07_

---

_Label `autofix` added by @zanieb on 2023-10-21 16:07_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-11-05 10:59_

---

_Closed by @dhruvmanila on 2023-11-09 03:10_

---
