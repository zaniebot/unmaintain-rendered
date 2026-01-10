```yaml
number: 20937
title: "[ty] Re-enable fuzzer seeds that are no longer slow"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/fast-fuzzer
created_at: 2025-10-17T11:23:16Z
updated_at: 2025-10-17T11:40:22Z
url: https://github.com/astral-sh/ruff/pull/20937
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Re-enable fuzzer seeds that are no longer slow

---

_Pull request opened by @AlexWaygood on 2025-10-17 11:23_

## Summary

And set a much smaller timeout so that it becomes obvious if any seeds become pathologically slow again

## Test Plan

CI on this PR


---

_Label `ty` added by @AlexWaygood on 2025-10-17 11:23_

---

_Label `ci` added by @AlexWaygood on 2025-10-17 11:23_

---

_@MichaReiser reviewed on 2025-10-17 11:24_

---

_Review comment by @MichaReiser on `python/py-fuzzer/fuzz.py`:157 on 2025-10-17 11:24_

Nice, so we're back to that one failing seed (that panics)

---

_@MichaReiser approved on 2025-10-17 11:24_

---

_@AlexWaygood reviewed on 2025-10-17 11:25_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/fuzz.py`:157 on 2025-10-17 11:25_

Hopefully...!

---

_Marked ready for review by @AlexWaygood on 2025-10-17 11:29_

---

_Merged by @AlexWaygood on 2025-10-17 11:29_

---

_Closed by @AlexWaygood on 2025-10-17 11:29_

---

_Branch deleted on 2025-10-17 11:29_

---

_Comment by @github-actions[bot] on 2025-10-17 11:40_

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
