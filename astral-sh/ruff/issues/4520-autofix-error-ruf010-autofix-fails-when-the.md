```yaml
number: 4520
title: "[Autofix error] RUF010 autofix fails when the expression is in parentheses, e.g. `f\"{(str(a))}\"`"
type: issue
state: closed
author: konstin
labels:
  - fixes
assignees: []
created_at: 2023-05-19T08:10:45Z
updated_at: 2023-05-19T19:05:53Z
url: https://github.com/astral-sh/ruff/issues/4520
synced_at: 2026-01-12T15:54:44Z
```

# [Autofix error] RUF010 autofix fails when the expression is in parentheses, e.g. `f"{(str(a))}"`

---

_@konstin_

RUF010 creates an invalid autofix when the expression is already in parentheses, e.g.
```python
f"{(str(a))}"
```
is replaced by 
```python
f"{(a!s)}"
```
which is invalid syntax.

Command:

```
ruff --no-cache --select RUF010 --fix code.py
```

ruff version fd16d658e9407ce76bc38cd8f5112dd272fc5be0

---

_Label `autofix` added by @konstin on 2023-05-19 08:10_

---

_Comment by @JonathanPlasse on 2023-05-19 08:25_

This version of `RUF010` using `libcst` should not have this problem.
[`ec263ce` (#4423)](https://github.com/charliermarsh/ruff/pull/4423/commits/ec263ceeb2e0800a22739f62b79e39defb56828d)

---

_Comment by @JonathanPlasse on 2023-05-19 08:37_

I will make a PR soon.

---

_Closed by @charliermarsh on 2023-05-19 19:05_

---
