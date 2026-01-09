---
number: 15221
title: "False positive for `RUF057` with negative ndigits"
type: issue
state: closed
author: UnknownPlatypus
labels:
  - bug
  - preview
assignees: []
created_at: 2025-01-02T13:04:04Z
updated_at: 2025-01-03T18:48:04Z
url: https://github.com/astral-sh/ruff/issues/15221
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive for `RUF057` with negative ndigits

---

_Issue opened by @UnknownPlatypus on 2025-01-02 13:04_

ruff propose this change:

```diff
-round(150, -2)
+150
```

which is not valid since `round(150, -2) == 200`

I believe negative ndigits should be skipped

Related to #14828

---

_Comment by @MichaReiser on 2025-01-02 14:03_

CC: @InSyncWithFoo 

---

_Label `bug` added by @MichaReiser on 2025-01-02 14:03_

---

_Label `preview` added by @MichaReiser on 2025-01-02 14:03_

---

_Comment by @InSyncWithFoo on 2025-01-02 15:55_

Got it. Fix coming tomorrow.

---

_Comment by @MichaReiser on 2025-01-02 15:57_

Awesome, thank you

---

_Assigned to @InSyncWithFoo by @MichaReiser on 2025-01-02 15:57_

---

_Referenced in [astral-sh/ruff#15234](../../astral-sh/ruff/pulls/15234.md) on 2025-01-03 05:06_

---

_Closed by @MichaReiser on 2025-01-03 18:48_

---
