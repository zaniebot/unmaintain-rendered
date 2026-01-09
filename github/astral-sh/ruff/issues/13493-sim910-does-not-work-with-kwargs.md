---
number: 13493
title: "SIM910 does not work with **kwargs"
type: issue
state: closed
author: spaceby
labels:
  - rule
assignees: []
created_at: 2024-09-24T08:04:43Z
updated_at: 2024-09-25T15:03:00Z
url: https://github.com/astral-sh/ruff/issues/13493
synced_at: 2026-01-07T13:12:15-06:00
---

# SIM910 does not work with **kwargs

---

_Issue opened by @spaceby on 2024-09-24 08:04_

SIM910 does not work with **kwargs

```python
def foo(**kwargs):
    # SIM910 not working
    a = kwargs.get('a', None)
```
https://play.ruff.rs/7f269c7d-e859-4672-b4ca-02d59036de87


---

_Label `rule` added by @MichaReiser on 2024-09-24 10:19_

---

_Comment by @MichaReiser on 2024-09-24 10:23_

Thanks. 

Ruff doesn't seem to understand that `**kwargs` is a dictionary because `is_dict` returns `false`

https://github.com/astral-sh/ruff/blob/6963f75a14ac58886adf7302745fcda2b9fd3f21/crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs#L276-L286

We should either improve our type inference to infer `dict` for `kwargs` or use our semantic model API to test whether the binding is a `**kwargs` argument.

---

_Assigned to @zanieb by @zanieb on 2024-09-24 14:03_

---

_Referenced in [astral-sh/ruff#13503](../../astral-sh/ruff/pulls/13503.md) on 2024-09-24 14:10_

---

_Closed by @zanieb on 2024-09-25 15:03_

---
