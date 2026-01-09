---
number: 16773
title: RUF040 misses list literal
type: issue
state: open
author: de-odex
labels:
  - rule
  - wish
assignees: []
created_at: 2025-03-16T08:42:59Z
updated_at: 2025-03-16T08:55:59Z
url: https://github.com/astral-sh/ruff/issues/16773
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF040 misses list literal

---

_Issue opened by @de-odex on 2025-03-16 08:42_

### Summary

this code doesn't seem to trigger RUF040:
```python
assert ["aaa", "111"], ["abc", "123"]
```
https://play.ruff.rs/aa462838-caaa-4b6c-8503-4bb09c238d5a

i'm not sure if this is intentional.

### Version

ruff 0.11.0 (2cd25ef64 2025-03-14)

---

_Comment by @MichaReiser on 2025-03-16 08:55_

We intentionally started with a limited set of expressions (https://github.com/astral-sh/ruff/pull/14488) but I think it makes sense to extend the rule to also catch list, tuple, and dict expressions. 

---

_Label `rule` added by @MichaReiser on 2025-03-16 08:55_

---

_Label `wish` added by @MichaReiser on 2025-03-16 08:55_

---
