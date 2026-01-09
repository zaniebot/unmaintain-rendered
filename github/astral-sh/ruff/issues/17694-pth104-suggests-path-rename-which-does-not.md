---
number: 17694
title: "`PTH104` suggests `Path.rename()` which does not support file descriptors"
type: issue
state: closed
author: sbrudenell
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-04-28T21:41:03Z
updated_at: 2025-05-01T14:01:18Z
url: https://github.com/astral-sh/ruff/issues/17694
synced_at: 2026-01-07T13:12:16-06:00
---

# `PTH104` suggests `Path.rename()` which does not support file descriptors

---

_Issue opened by @sbrudenell on 2025-04-28 21:41_

### Summary

```python
import os
os.rename("src", "dst", src_dir_fd=3, dst_dir_fd=4)
```
```shell
$ ruff --version
ruff 0.11.7
$ ruff check --isolated --select PTH104 t.py
t.py:2:1: PTH104 `os.rename()` should be replaced by `Path.rename()`
  |
1 | import os
2 | os.rename("src", "dst", src_dir_fd=3, dst_dir_fd=4)
  | ^^^^^^^^^ PTH104
  |

Found 1 error.
```

`Path.rename()` has no support for `dir_fd` arguments.

Related: #17693, #12871, #7620

### Version

0.11.7

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

_Referenced in [astral-sh/ruff#17712](../../astral-sh/ruff/pulls/17712.md) on 2025-04-29 14:50_

---

_Closed by @ntBre on 2025-05-01 14:01_

---
