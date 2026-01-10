```yaml
number: 18624
title: "C414: additional unnecessary double casts"
type: issue
state: open
author: xmo-odoo
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-11T11:00:50Z
updated_at: 2025-06-11T11:03:53Z
url: https://github.com/astral-sh/ruff/issues/18624
synced_at: 2026-01-10T11:09:58Z
```

# C414: additional unnecessary double casts

---

_Issue opened by @xmo-odoo on 2025-06-11 11:00_

### Summary

Running C4 on a pretty old codebase, I found a few invocations which left a few double casts of various forms which might be removable by C414:

- `collections.Counter`, takes an arbitrary iterable, so `collections.Counter(list(...))` is redundant
- `str.split` returns a list, so e.g. `list(str.split(...))` is redundant
- `str.join` takes an arbitrary iterable, so `str.join(list(...))` is redundant
- `dict(dict.items())` is just `dict(items)`, I think

---

_Label `rule` added by @ntBre on 2025-06-11 11:03_

---

_Label `needs-decision` added by @ntBre on 2025-06-11 11:03_

---
