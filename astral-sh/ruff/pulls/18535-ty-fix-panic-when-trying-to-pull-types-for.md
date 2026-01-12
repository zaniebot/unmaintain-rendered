```yaml
number: 18535
title: "[ty] Fix panic when trying to pull types for attribute expressions inside `Literal` type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/annotationlib-crash
created_at: 2025-06-07T14:55:42Z
updated_at: 2025-06-11T10:48:20Z
url: https://github.com/astral-sh/ruff/pull/18535
synced_at: 2026-01-12T15:56:20Z
```

# [ty] Fix panic when trying to pull types for attribute expressions inside `Literal` type expressions

---

_@AlexWaygood_

## Summary

We weren't storing the type for attribute expressions inside `Literal` slices. This was causing us to panic when pulling types on four typeshed files.

## Test Plan

Added a corpus test that previously failed and removed four entries from the `KNOWN_FAILURES` corpus test ignorelist. All existing tests pass.

---

_Label `ty` added by @AlexWaygood on 2025-06-07 14:55_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-07 14:55_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-07 14:55_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-07 14:55_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-07 14:55_

---

_Comment by @github-actions[bot] on 2025-06-07 14:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-06-07 14:59_

---

_Closed by @AlexWaygood on 2025-06-07 14:59_

---

_Branch deleted on 2025-06-07 14:59_

---

_Label `bug` added by @dhruvmanila on 2025-06-11 10:48_

---
