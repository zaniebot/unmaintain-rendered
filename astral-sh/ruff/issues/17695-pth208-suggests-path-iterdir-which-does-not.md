```yaml
number: 17695
title: "`PTH208` suggests `Path.iterdir()`, which does not support file descriptors"
type: issue
state: closed
author: sbrudenell
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-04-28T21:48:12Z
updated_at: 2025-04-30T14:38:58Z
url: https://github.com/astral-sh/ruff/issues/17695
synced_at: 2026-01-12T15:54:56Z
```

# `PTH208` suggests `Path.iterdir()`, which does not support file descriptors

---

_@sbrudenell_

### Summary

```python
import os
os.listdir(1)
```
```shell
$ ruff --version
ruff 0.11.7
$ ruff check --isolated --select PTH208 t.py
t.py:2:1: PTH208 Use `pathlib.Path.iterdir()` instead.
  |
1 | import os
2 | os.listdir(1)
  | ^^^^^^^^^^ PTH208
  |

Found 1 error.
```

`Path.iterdir()` doesn't support directory file descriptors.

Related: #17694, #17693, #17691, #12871, #7620

### Version

0.11.7

---

_Label `bug` added by @MichaReiser on 2025-04-29 06:13_

---

_Label `rule` added by @MichaReiser on 2025-04-29 06:13_

---

_Label `help wanted` added by @MichaReiser on 2025-04-29 06:13_

---

_Closed by @dhruvmanila on 2025-04-30 14:38_

---

_Closed by @dhruvmanila on 2025-04-30 14:38_

---
