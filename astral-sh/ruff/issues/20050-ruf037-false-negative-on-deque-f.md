---
number: 20050
title: "RUF037 false negative on `deque(f\"{\"\"}\")`"
type: issue
state: closed
author: dscorbett
labels:
  - good first issue
  - rule
assignees: []
created_at: 2025-08-22T19:02:29Z
updated_at: 2025-08-28T20:58:40Z
url: https://github.com/astral-sh/ruff/issues/20050
synced_at: 2026-01-10T01:23:01Z
---

# RUF037 false negative on `deque(f"{""}")`

---

_Issue opened by @dscorbett on 2025-08-22 19:02_

### Summary

[`unnecessary-empty-iterable-within-deque-call` (RUF037)](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/) checks whether an f-string is empty in the sense of being the empty literal `f""`, whereas it should should check whether it evaluates to an empty iterable. That is, it should be implemented in terms of `is_empty_f_string` instead of `is_empty_literal`. Example:
```console
$ cat >ruf037.py <<'# EOF'
from collections import deque
deque(f"{""}")
# EOF

$ ruff --isolated check ruf037.py --select RUF037 --preview
All checks passed!
```

### Version

ruff 0.12.10 (c68ff8d90 2025-08-21)

---

_Label `rule` added by @dylwil3 on 2025-08-22 19:51_

---

_Label `help wanted` added by @dylwil3 on 2025-08-22 20:28_

---

_Label `help wanted` removed by @dylwil3 on 2025-08-22 20:28_

---

_Label `good first issue` added by @dylwil3 on 2025-08-22 20:28_

---

_Referenced in [astral-sh/ruff#20109](../../astral-sh/ruff/pulls/20109.md) on 2025-08-27 03:04_

---

_Closed by @ntBre on 2025-08-28 20:58_

---
