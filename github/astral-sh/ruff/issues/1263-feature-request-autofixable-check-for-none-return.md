---
number: 1263
title: "Feature request: autofixable check for `-> None` return of `__init__(self)` with no other arguments"
type: issue
state: closed
author: tmke8
labels: []
assignees: []
created_at: 2022-12-16T13:35:52Z
updated_at: 2022-12-16T20:15:35Z
url: https://github.com/astral-sh/ruff/issues/1263
synced_at: 2026-01-07T13:12:14-06:00
---

# Feature request: autofixable check for `-> None` return of `__init__(self)` with no other arguments

---

_Issue opened by @tmke8 on 2022-12-16 13:35_

mypy has this somewhat silly rule that you have to annotate an `__init__` that doesn't have any arguments beyond `self` with `-> None`: https://mypy.readthedocs.io/en/stable/class_basics.html#annotating-init-methods

It would be great if ruff could automatically fix this!

---

_Comment by @charliermarsh on 2022-12-16 16:29_

Oh yeah, we can definitely do that. I think this would be considered an autofix of `ANN204`.


---

_Label `enhancement` added by @charliermarsh on 2022-12-16 17:33_

---

_Referenced in [astral-sh/ruff#1265](../../astral-sh/ruff/pulls/1265.md) on 2022-12-16 19:28_

---

_Closed by @charliermarsh on 2022-12-16 19:28_

---

_Comment by @charliermarsh on 2022-12-16 19:30_

Let me know if this works for you once the next release is out. You may also want to set:

```toml
[tool.ruff.flake8-annotations]
mypy-init-return = true
```

Otherwise, it'll _always_ add `-> None` to `__init__` (instead of only doing so when it has no typed arguments).


---

_Comment by @tmke8 on 2022-12-16 19:43_

Will do!

---

_Comment by @charliermarsh on 2022-12-16 20:15_

Should be live in [v0.0.184](https://github.com/charliermarsh/ruff/releases/tag/v0.0.184) :)

---
