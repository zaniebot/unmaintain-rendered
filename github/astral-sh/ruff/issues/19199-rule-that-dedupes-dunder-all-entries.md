---
number: 19199
title: Rule that dedupes dunder-all entries
type: issue
state: open
author: zzJinux
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-08T09:32:40Z
updated_at: 2025-07-08T12:49:22Z
url: https://github.com/astral-sh/ruff/issues/19199
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule that dedupes dunder-all entries

---

_Issue opened by @zzJinux on 2025-07-08 09:32_

### Summary

```
# before

__all__ = ["a", "b", "c", "b"]

# after

__all__ = ["a", "b", "c"]
```

Only the first appearing one survives.

---

_Label `rule` added by @ntBre on 2025-07-08 12:48_

---

_Label `needs-decision` added by @ntBre on 2025-07-08 12:48_

---

_Comment by @ntBre on 2025-07-08 12:49_

Makes sense to me. I thought we had a rule like this, but the closest I could find were [duplicate-value (B033)](https://docs.astral.sh/ruff/rules/duplicate-value/#duplicate-value-b033) and [unsorted-dunder-all (RUF022)](https://docs.astral.sh/ruff/rules/unsorted-dunder-all/#unsorted-dunder-all-ruf022).

---
