```yaml
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
synced_at: 2026-01-12T15:54:56Z
```

# `PTH104` suggests `Path.rename()` which does not support file descriptors

---

_@sbrudenell_

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

_Label `bug` added by @MichaReiser on 2025-04-29 06:12_

---

_Label `rule` added by @MichaReiser on 2025-04-29 06:12_

---

_Label `help wanted` added by @MichaReiser on 2025-04-29 06:12_

---

_Closed by @ntBre on 2025-05-01 14:01_

---
