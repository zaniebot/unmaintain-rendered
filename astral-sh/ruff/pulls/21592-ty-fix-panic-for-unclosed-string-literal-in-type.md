```yaml
number: 21592
title: "[ty] Fix panic for unclosed string literal in type annotation position"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/fix-overflow-closer-len
created_at: 2025-11-23T11:57:11Z
updated_at: 2025-11-23T15:52:00Z
url: https://github.com/astral-sh/ruff/pull/21592
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix panic for unclosed string literal in type annotation position

---

_Pull request opened by @MichaReiser on 2025-11-23 11:57_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1611

## Test Plan

Added test


---

_Label `bug` added by @MichaReiser on 2025-11-23 11:57_

---

_Label `ty` added by @MichaReiser on 2025-11-23 11:57_

---

_Review requested from @carljm by @MichaReiser on 2025-11-23 11:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-11-23 11:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-23 11:57_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-23 11:57_

---

_Label `bug` added by @MichaReiser on 2025-11-23 11:57_

---

_Label `ty` added by @MichaReiser on 2025-11-23 11:57_

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 12:19_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@AlexWaygood approved on 2025-11-23 15:20_

Nice fix!

---

_Comment by @MichaReiser on 2025-11-23 15:22_

> Nice fix!

Thanks to your foresight for adding this method in the first place!

---

_Merged by @MichaReiser on 2025-11-23 15:51_

---

_Closed by @MichaReiser on 2025-11-23 15:51_

---

_Branch deleted on 2025-11-23 15:52_

---
