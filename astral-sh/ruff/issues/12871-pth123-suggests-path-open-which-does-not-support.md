```yaml
number: 12871
title: "`PTH123` suggests `Path.open()` which does not support file descriptors"
type: issue
state: closed
author: sbrudenell
labels:
  - bug
  - rule
assignees: []
created_at: 2024-08-13T23:49:55Z
updated_at: 2024-10-04T13:48:48Z
url: https://github.com/astral-sh/ruff/issues/12871
synced_at: 2026-01-12T15:54:52Z
```

# `PTH123` suggests `Path.open()` which does not support file descriptors

---

_@sbrudenell_

```python
fp = open(2)
```
```bash
$ ruff --version
ruff 0.5.7
$ ruff check --isolated --select PTH123 t.py
t.py:1:6: PTH123 `open()` should be replaced by `Path.open()`
  |
1 | fp = open(2)
  |      ^^^^ PTH123
  |

Found 1 error.
```
`Path.open()` doesn't seem to have a variant which supports a file descriptor.

This is essentially similar to #7620

---

_Label `bug` added by @MichaReiser on 2024-08-14 07:24_

---

_Label `rule` added by @MichaReiser on 2024-08-14 07:24_

---

_Comment by @MichaReiser on 2024-08-14 07:24_

Makes sense. Thanks

---

_Closed by @zanieb on 2024-10-04 13:48_

---
