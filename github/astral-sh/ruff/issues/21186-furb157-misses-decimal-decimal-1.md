---
number: 21186
title: "FURB157 misses `decimal.Decimal(\"_-1\")`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-11-01T17:03:22Z
updated_at: 2025-11-04T16:02:52Z
url: https://github.com/astral-sh/ruff/issues/21186
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB157 misses `decimal.Decimal("_-1")`

---

_Issue opened by @dscorbett on 2025-11-01 17:03_

### Summary

[`verbose-decimal-constructor` (FURB157)](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor/) has a false negative when the sign character follows an underscore. [Example](https://play.ruff.rs/34abd766-bcd1-4e22-9a21-aa2d340f859f):
```console
$ cat >furb157.py <<'# EOF'
from decimal import Decimal
Decimal("_-1")
# EOF

$ ruff --isolated check furb157.py --select FURB157
All checks passed!
```
This was a true positive in version 0.14.2.

### Version

ruff 0.14.3 (8737a2d5f 2025-10-30)

---

_Referenced in [astral-sh/ruff#21190](../../astral-sh/ruff/pulls/21190.md) on 2025-11-01 19:26_

---

_Label `bug` added by @ntBre on 2025-11-03 18:27_

---

_Label `rule` added by @ntBre on 2025-11-03 18:27_

---

_Closed by @ntBre on 2025-11-04 16:02_

---
