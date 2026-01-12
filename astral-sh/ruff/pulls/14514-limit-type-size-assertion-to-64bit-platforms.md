```yaml
number: 14514
title: Limit type size assertion to 64bit platforms
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-32-bit-buidl
created_at: 2024-11-21T12:45:25Z
updated_at: 2024-11-21T13:08:39Z
url: https://github.com/astral-sh/ruff/pull/14514
synced_at: 2026-01-12T15:55:48Z
```

# Limit type size assertion to 64bit platforms

---

_@MichaReiser_

## Summary

Fix 32bit release builds by limiting the `Type` size assertion to 64bit architectures. 

32 and 64 bit architectures have different sizes because the size of a string slice differs (because of pointer size)




---

_Review requested from @carljm by @MichaReiser on 2024-11-21 12:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-21 12:45_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-21 12:45_

---

_Label `red-knot` added by @MichaReiser on 2024-11-21 12:45_

---

_Renamed from "Limit type size assertion to 64bit" to "Limit type size assertion to 64bit platforms" by @MichaReiser on 2024-11-21 12:47_

---

_Merged by @MichaReiser on 2024-11-21 12:49_

---

_Closed by @MichaReiser on 2024-11-21 12:49_

---

_Branch deleted on 2024-11-21 12:49_

---

_Comment by @github-actions[bot] on 2024-11-21 12:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2024-11-21 13:08_

Thanks

---
