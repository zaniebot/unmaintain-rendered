```yaml
number: 9282
title: explicit-preview-rules does not work for fixes
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
assignees: []
created_at: 2023-12-26T08:08:04Z
updated_at: 2024-01-16T02:48:42Z
url: https://github.com/astral-sh/ruff/issues/9282
synced_at: 2026-01-10T11:09:51Z
```

# explicit-preview-rules does not work for fixes

---

_Issue opened by @hauntsaninja on 2023-12-26 08:08_

I'd expect both runs of ruff below to fix the error
```
~/tmp/repro λ ruff --version
ruff 0.1.9

~/tmp/repro λ cat asdf.py
lists = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
sum(lists, [])
~/tmp/repro λ cat pyproject.toml
[tool.ruff]
select = ["RUF017"]
[tool.ruff.lint]
preview = true
explicit-preview-rules = true
~/tmp/repro λ ruff asdf.py --fix --unsafe-fixes
asdf.py:2:1: RUF017 Avoid quadratic list summation
Found 1 error.

~/tmp/repro λ vim pyproject.toml
~/tmp/repro λ cat pyproject.toml
[tool.ruff]
select = ["RUF017"]
[tool.ruff.lint]
preview = true
~/tmp/repro λ ruff asdf.py --fix --unsafe-fixes
Found 1 error (1 fixed, 0 remaining).
```

---

_Renamed from "explicit-preview-rules does not work for rules explicitly selected on command line" to "explicit-preview-rules does not work for fixes" by @hauntsaninja on 2023-12-26 08:09_

---

_Comment by @zanieb on 2023-12-26 17:59_

Thanks for the report! My assumption is that the `fixable` setting which defaults to `ALL` is not including the preview rule since it is not explicit. We should probably exempt `fixable` from these semantics.

---

_Label `bug` added by @zanieb on 2023-12-26 17:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-15 20:11_

---

_Closed by @charliermarsh on 2024-01-16 02:48_

---
