---
number: 17693
title: "`PTH116` suggests `Path.stat()` which does not support file descriptors"
type: issue
state: closed
author: sbrudenell
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-04-28T21:35:14Z
updated_at: 2025-05-01T06:16:29Z
url: https://github.com/astral-sh/ruff/issues/17693
synced_at: 2026-01-07T13:12:16-06:00
---

# `PTH116` suggests `Path.stat()` which does not support file descriptors

---

_Issue opened by @sbrudenell on 2025-04-28 21:35_

### Summary

```python
import os
os.stat(1)
```
```shell
$ ruff --version
ruff 0.11.7
$ ruff check --isolated --select PTH116 t.py
t.py:2:1: PTH116 `os.stat()` should be replaced by `Path.stat()`, `Path.owner()`, or `Path.group()`
  |
1 | import os
2 | os.stat(1)
  | ^^^^^^^ PTH116
  |

Found 1 error.
```
`Path.stat()` has no support for file descriptors.

Related: #12871, #7620

### Version

0.11.7

---

_Referenced in [astral-sh/ruff#17694](../../astral-sh/ruff/issues/17694.md) on 2025-04-28 21:41_

---

_Referenced in [astral-sh/ruff#17695](../../astral-sh/ruff/issues/17695.md) on 2025-04-28 21:48_

---

_Referenced in [astral-sh/ruff#17699](../../astral-sh/ruff/issues/17699.md) on 2025-04-29 06:10_

---

_Label `bug` added by @MichaReiser on 2025-04-29 06:12_

---

_Label `rule` added by @MichaReiser on 2025-04-29 06:12_

---

_Label `help wanted` added by @MichaReiser on 2025-04-29 06:12_

---

_Referenced in [astral-sh/ruff#17709](../../astral-sh/ruff/pulls/17709.md) on 2025-04-29 14:17_

---

_Closed by @MichaReiser on 2025-05-01 06:16_

---

_Referenced in [astral-sh/ruff#17776](../../astral-sh/ruff/issues/17776.md) on 2025-05-01 19:04_

---
