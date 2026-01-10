---
number: 9663
title: PIE810 autofix changes raw strings to backslashes
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-01-28T14:47:41Z
updated_at: 2025-01-23T17:12:11Z
url: https://github.com/astral-sh/ruff/issues/9663
synced_at: 2026-01-10T01:22:49Z
---

# PIE810 autofix changes raw strings to backslashes

---

_Issue opened by @JelleZijlstra on 2024-01-28 14:47_

Start with this code:

```python
if x.startswith("a") or x.startswith("b") or re.match(r"a\.b", x):
    pass
```

Ruff correctly complains that I should use a single `.startswith()` call.

Run Ruff's autofix for PIE810:

```python
if x.startswith(("a", "b")) or re.match("a\\.b", x):
    pass
```

And now my string is not raw any more.

---

_Comment by @charliermarsh on 2024-01-28 15:36_

👍 This is a specific instance of a general problem (#7799), though we may want to fix this case specifically since it's common for this to contain strings.

---

_Label `bug` added by @charliermarsh on 2024-01-28 15:36_

---

_Label `autofix` added by @charliermarsh on 2024-01-28 15:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-28 19:04_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-01-28 19:31_

---

_Referenced in [astral-sh/ruff#10314](../../astral-sh/ruff/pulls/10314.md) on 2024-03-10 10:12_

---

_Assigned to @ntBre by @ntBre on 2025-01-23 14:57_

---

_Referenced in [astral-sh/ruff#15694](../../astral-sh/ruff/pulls/15694.md) on 2025-01-23 15:40_

---

_Closed by @ntBre on 2025-01-23 17:12_

---

_Referenced in [ArduPilot/ardupilot#30343](../../ArduPilot/ardupilot/pulls/30343.md) on 2025-06-14 09:04_

---
