---
number: 13771
title: "B007 fixer: don't mark loop variable as ignored when the dict iteration can be fixed"
type: issue
state: closed
author: xmo-odoo
labels: []
assignees: []
created_at: 2024-10-16T09:22:43Z
updated_at: 2024-10-17T09:34:47Z
url: https://github.com/astral-sh/ruff/issues/13771
synced_at: 2026-01-07T13:12:15-06:00
---

# B007 fixer: don't mark loop variable as ignored when the dict iteration can be fixed

---

_Issue opened by @xmo-odoo on 2024-10-16 09:22_

Given code like
```python
for a, b in foo.items():
    something(b)
```
Currently the fixer will rename `a` to `_a`, but the better fix here would be to drop `a` entirely and iterate on `foo.values()` instead. Likewise `foo.keys()` if `b` is the unused loop variable.

Now obviously `foo` could technically be a non-mapping which just happens to have a `.items()` yielding a pair of items, but given the fixer is already unsafe I am not sure that would be a major issue?

---

_Comment by @sbrugman on 2024-10-16 22:10_

Hi @xmo-odoo ,

Thanks for raising this issue.

The rule [incorrect-dict-iterator (PERF102)](https://docs.astral.sh/ruff/rules/incorrect-dict-iterator/#incorrect-dict-iterator-perf102) already addresses the case you are describing (and supports auto fix which is marked unsafe as you note).



---

_Closed by @xmo-odoo on 2024-10-17 09:34_

---

_Comment by @xmo-odoo on 2024-10-17 09:34_

Noted, thanks.

---
