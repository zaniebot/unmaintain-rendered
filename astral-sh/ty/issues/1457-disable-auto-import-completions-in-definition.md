```yaml
number: 1457
title: Disable auto-import completions in definition places
type: issue
state: closed
author: MichaReiser
labels:
  - server
  - completions
assignees: []
created_at: 2025-10-30T14:46:45Z
updated_at: 2025-10-30T23:20:00Z
url: https://github.com/astral-sh/ty/issues/1457
synced_at: 2026-01-12T15:54:25Z
```

# Disable auto-import completions in definition places

---

_@MichaReiser_

### Summary

I don't want to import `math.fabs` when my cursor is positioned at `def f<CURSOR>` or `class f<CURSOR>`. 

We may want to offer some completions for functions when within a class to override a method, but that's something we can track separately 

### Version

_No response_

---

_Label `completions` added by @MichaReiser on 2025-10-30 14:46_

---

_Label `server` added by @AlexWaygood on 2025-10-30 14:49_

---

_Closed by @MichaReiser on 2025-10-30 23:20_

---
