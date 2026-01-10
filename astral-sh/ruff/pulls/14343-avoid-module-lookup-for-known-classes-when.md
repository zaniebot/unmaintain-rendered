```yaml
number: 14343
title: Avoid module lookup for known classes when possible
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/known-class-avoid-module-lookup
created_at: 2024-11-14T20:02:32Z
updated_at: 2024-11-14T20:24:13Z
url: https://github.com/astral-sh/ruff/pull/14343
synced_at: 2026-01-10T20:50:57Z
```

# Avoid module lookup for known classes when possible

---

_Pull request opened by @MichaReiser on 2024-11-14 20:02_

## Summary

Avoid calling `file_to_module` when resolving known classes unless 
the class name matches the name of a known class. 

I don't expect this to change performance in a significant way (there aren't enough classes)


## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-14 20:02_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-14 20:02_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-14 20:02_

---

_Label `red-knot` added by @MichaReiser on 2024-11-14 20:02_

---

_Comment by @github-actions[bot] on 2024-11-14 20:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-11-14 20:24_

---

_Closed by @MichaReiser on 2024-11-14 20:24_

---

_Branch deleted on 2024-11-14 20:24_

---
