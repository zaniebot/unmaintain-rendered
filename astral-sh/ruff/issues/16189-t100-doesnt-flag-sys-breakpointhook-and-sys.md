```yaml
number: 16189
title: "T100 doesn’t flag `sys.breakpointhook` and `sys.__breakpointhook__`"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-02-16T16:03:02Z
updated_at: 2025-02-16T19:50:18Z
url: https://github.com/astral-sh/ruff/issues/16189
synced_at: 2026-01-12T15:54:55Z
```

# T100 doesn’t flag `sys.breakpointhook` and `sys.__breakpointhook__`

---

_@dscorbett_

### Description

[`debugger` (T100)](https://docs.astral.sh/ruff/rules/debugger/) doesn’t flag `sys.breakpointhook` and `sys.__breakpointhook__` in Ruff 0.9.6. It should, because T100 does flag `breakpoint`, which just calls `sys.breakpointhook`, and there is no reason to treat them differently. Also, Pylint’s [`forgotten-debug-statement` (W1515)](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/forgotten-debug-statement.html), which in #970 is mapped to T100, flags `sys.breakpointhook`.

---

_Label `rule` added by @ntBre on 2025-02-16 17:27_

---

_Assigned to @ntBre by @ntBre on 2025-02-16 17:39_

---

_Closed by @ntBre on 2025-02-16 19:50_

---
