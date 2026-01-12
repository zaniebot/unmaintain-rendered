```yaml
number: 20594
title: Quick fixes are unavailable in the playground for empty ranges
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - playground
assignees: []
created_at: 2025-09-26T13:43:59Z
updated_at: 2025-09-29T07:38:33Z
url: https://github.com/astral-sh/ruff/issues/20594
synced_at: 2026-01-12T15:54:57Z
```

# Quick fixes are unavailable in the playground for empty ranges

---

_@dscorbett_

### Summary

Quick fixes are only available in the playground when the cursor is within the range of code to be fixed. This makes it impossible to apply a quick fix to an empty range. [Here is an example with W292.](https://play.ruff.rs/c80ed5e7-347b-495a-a7a4-8d4adc102132) This used to work in previous versions.

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Label `bug` added by @MichaReiser on 2025-09-26 13:49_

---

_Label `playground` added by @MichaReiser on 2025-09-26 13:49_

---

_Comment by @MichaReiser on 2025-09-26 13:50_

@danparizher this might be due to your most recent changes. Are you interested in taking a look at this?

---

_Comment by @danparizher on 2025-09-26 14:13_

Sure thing, feel free to assign it to me, thanks!

---

_Assigned to @danparizher by @MichaReiser on 2025-09-26 14:17_

---

_Closed by @MichaReiser on 2025-09-29 07:38_

---
