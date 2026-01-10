```yaml
number: 17776
title: "PTH116 should be suppressed when `dir_fd` is set"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-05-01T19:04:10Z
updated_at: 2025-05-12T20:11:57Z
url: https://github.com/astral-sh/ruff/issues/17776
synced_at: 2026-01-10T11:09:58Z
```

# PTH116 should be suppressed when `dir_fd` is set

---

_Issue opened by @dscorbett on 2025-05-01 19:04_

### Summary

[`os-stat` (PTH116)](https://docs.astral.sh/ruff/rules/os-stat/) should be suppressed when `dir_fd` is set to a non-`None` value because the suggested `Path` methods don’t support file descriptors. This is a follow-up to #17693.

```console
$ cat >pth116.py <<'# EOF'
import os
os.stat("x", dir_fd=3)
# EOF

$ ruff --isolated check --select PTH116 pth116.py
pth116.py:2:1: PTH116 `os.stat()` should be replaced by `Path.stat()`, `Path.owner()`, or `Path.group()`
  |
1 | import os
2 | os.stat("x", dir_fd=3)
  | ^^^^^^^ PTH116
  |

Found 1 error.
```

### Version

ruff 0.11.8 (75effb8ed 2025-05-01)

---

_Label `bug` added by @ntBre on 2025-05-01 19:28_

---

_Label `rule` added by @ntBre on 2025-05-01 19:28_

---

_Label `help wanted` added by @ntBre on 2025-05-01 19:28_

---

_Comment by @ntBre on 2025-05-01 19:28_

It looks like many of the `os` methods accept a `dir_fd` or similar argument, so we may want to check the other `PTH` rules for this behavior too.

---

_Comment by @LaBatata101 on 2025-05-07 21:08_

I can take this one

---

_Assigned to @LaBatata101 by @ntBre on 2025-05-07 21:47_

---

_Closed by @ntBre on 2025-05-12 20:11_

---
