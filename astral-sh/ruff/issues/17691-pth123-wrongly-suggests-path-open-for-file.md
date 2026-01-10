```yaml
number: 17691
title: "`PTH123` wrongly suggests `Path.open()` for file descriptors, only sometimes"
type: issue
state: closed
author: sbrudenell
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-04-28T21:30:16Z
updated_at: 2025-04-29T20:51:39Z
url: https://github.com/astral-sh/ruff/issues/17691
synced_at: 2026-01-10T11:09:58Z
```

# `PTH123` wrongly suggests `Path.open()` for file descriptors, only sometimes

---

_Issue opened by @sbrudenell on 2025-04-28 21:30_

### Summary

```python
fp = open(1)
def f() -> int:
    return 1
fp = open(f())
def g(x: int) -> None:
    open(x)
r = 1
fp = open(r)
```
```zsh
$ ruff --version
ruff 0.11.7
$ ruff check --isolated --select PTH123 t.py
t.py:4:6: PTH123 `open()` should be replaced by `Path.open()`
  |
2 | def f() -> int:
3 |     return 1
4 | fp = open(f())
  |      ^^^^ PTH123
5 | def g(x: int) -> None:
6 |     open(x)
  |

Found 1 error.
```

This doesn't look like a regression of #12871. But I guess the fix (#13616) somehow didn't cover the case where the argument of `open()` is an `int` return value?

### Version

0.11.7

---

_Label `bug` added by @MichaReiser on 2025-04-29 06:12_

---

_Label `rule` added by @MichaReiser on 2025-04-29 06:12_

---

_Label `help wanted` added by @MichaReiser on 2025-04-29 06:12_

---

_Closed by @ntBre on 2025-04-29 20:51_

---

_Closed by @ntBre on 2025-04-29 20:51_

---
