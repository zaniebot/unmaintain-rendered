```yaml
number: 12827
title: uv remove should support inexact
type: issue
state: open
author: wu-clan
labels:
  - enhancement
assignees: []
created_at: 2025-04-11T07:04:53Z
updated_at: 2025-04-16T15:20:30Z
url: https://github.com/astral-sh/uv/issues/12827
synced_at: 2026-01-12T16:01:13Z
```

# uv remove should support inexact

---

_@wu-clan_

### Summary

Currently, you can retain extra packages using `uv sync --frozen --inexact`, but `uv remove` does not support `inexact`, so it will still delete extra packages during automatic sync.

### Example

_No response_

---

_Label `enhancement` added by @wu-clan on 2025-04-11 07:04_

---

_Comment by @johnslavik on 2025-04-16 12:04_

The issue title is a bit misleading. It can be read as "remove support for inexact".

---

_Renamed from "uv remove support inexact" to "uv remove should support inexact" by @wu-clan on 2025-04-16 14:34_

---

_Comment by @wu-clan on 2025-04-16 14:35_

@bswck Sorry, I've updated the title.

---

_Comment by @zanieb on 2025-04-16 15:20_

This seems reasonable to me, I think. Though `uv remove` has generally complicated behavior around removals that needs to be improved.

---
