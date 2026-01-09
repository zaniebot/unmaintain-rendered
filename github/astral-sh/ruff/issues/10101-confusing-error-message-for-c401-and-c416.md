---
number: 10101
title: Confusing error message for C401 and C416
type: issue
state: closed
author: runfalk
labels:
  - rule
assignees: []
created_at: 2024-02-23T16:47:07Z
updated_at: 2024-03-26T07:36:47Z
url: https://github.com/astral-sh/ruff/issues/10101
synced_at: 2026-01-07T13:12:15-06:00
---

# Confusing error message for C401 and C416

---

_Issue opened by @runfalk on 2024-02-23 16:47_

I ran into this oddity. The suggested fix for `C401` actually results in `C416` because `i` isn't transformed in any way. Is there a better way to indicate this in the error message?

```python
_a = set(i for i in range(10))
_b = {i for i in range(10)}
_c = set(range(10))
```

```
$ poetry run ruff check --output-format full test.py
test.py:1:6: C401 Unnecessary generator (rewrite as a `set` comprehension)
  |
1 | _a = set(i for i in range(10))
  |      ^^^^^^^^^^^^^^^^^^^^^^^^^ C401
2 | _b = {i for i in range(10)}
3 | _c = set(range(10))
  |
  = help: Rewrite as a `set` comprehension

test.py:2:6: C416 Unnecessary `set` comprehension (rewrite using `set()`)
  |
1 | _a = set(i for i in range(10))
2 | _b = {i for i in range(10)}
  |      ^^^^^^^^^^^^^^^^^^^^^^ C416
3 | _c = set(range(10))
  |
  = help: Rewrite using `set()`

Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

```
$ poetry run ruff --version
ruff 0.2.2
```

---

_Referenced in [astral-sh/ruff#10596](../../astral-sh/ruff/pulls/10596.md) on 2024-03-26 03:47_

---

_Comment by @charliermarsh on 2024-03-26 03:51_

Thanks for pointing this out -- fixed in https://github.com/astral-sh/ruff/pull/10596!

---

_Label `rule` added by @charliermarsh on 2024-03-26 03:51_

---

_Closed by @MichaReiser on 2024-03-26 07:36_

---
