```yaml
number: 14430
title: Add tests for python version compatibility
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: micha/tests-for-python-version-compatibility
created_at: 2024-11-18T11:51:09Z
updated_at: 2024-11-18T12:41:31Z
url: https://github.com/astral-sh/ruff/pull/14430
synced_at: 2026-01-12T15:55:47Z
```

# Add tests for python version compatibility

---

_@MichaReiser_

## Summary

We have multiple types that all represent `PythonVersion` and keeping them all in sync
is error-prone. This PR adds a few tests that should warn developers if they miss updating one `default` implementation.

## Test Plan

Added tests


---

_Review requested from @carljm by @MichaReiser on 2024-11-18 11:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-18 11:51_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-18 11:51_

---

_Label `testing` added by @MichaReiser on 2024-11-18 11:51_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-18 11:54_

---

_Label `red-knot` removed by @AlexWaygood on 2024-11-18 11:56_

---

_Label `internal` added by @AlexWaygood on 2024-11-18 11:56_

---

_@AlexWaygood approved on 2024-11-18 12:05_

---

_Comment by @github-actions[bot] on 2024-11-18 12:11_

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

_Merged by @MichaReiser on 2024-11-18 12:26_

---

_Closed by @MichaReiser on 2024-11-18 12:26_

---

_Branch deleted on 2024-11-18 12:26_

---
