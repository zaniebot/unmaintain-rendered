---
number: 19951
title: RUF037 does not support t-strings
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-08-18T02:02:40Z
updated_at: 2025-08-22T15:23:50Z
url: https://github.com/astral-sh/ruff/issues/19951
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF037 does not support t-strings

---

_Issue opened by @dscorbett on 2025-08-18 02:02_

### Summary

[`unnecessary-empty-iterable-within-deque-call` (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/) does not support t-strings. This is similar to #18854. [Example](https://play.ruff.rs/50213a4f-0610-4311-9f11-37ea1005748a):
```console
$ cat >ruf037.py <<'# EOF'
from collections import deque
queue = deque(t"")
# EOF

$ ruff --isolated check ruf037.py --select RUF037 --target-version py314 --preview
All checks passed!
```

### Version

ruff 0.12.9 (ef422460d 2025-08-14)

---

_Assigned to @dylwil3 by @dylwil3 on 2025-08-18 12:31_

---

_Label `rule` added by @dylwil3 on 2025-08-18 12:31_

---

_Referenced in [astral-sh/ruff#20045](../../astral-sh/ruff/pulls/20045.md) on 2025-08-22 15:01_

---

_Closed by @dylwil3 on 2025-08-22 15:23_

---
