```yaml
number: 18339
title: "[ty] Simplify `Type::normalized` slightly"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-normalize
created_at: 2025-05-27T18:06:01Z
updated_at: 2025-05-27T18:09:24Z
url: https://github.com/astral-sh/ruff/pull/18339
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Simplify `Type::normalized` slightly

---

_Pull request opened by @AlexWaygood on 2025-05-27 18:06_

## Summary

I added a `TypeVarInstance::normalized()` method in #18222 but missed that I could use it here as well

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-27 18:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-27 18:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-27 18:06_

---

_Label `internal` added by @AlexWaygood on 2025-05-27 18:06_

---

_Label `ty` added by @AlexWaygood on 2025-05-27 18:06_

---

_@carljm approved on 2025-05-27 18:08_

---

_Merged by @AlexWaygood on 2025-05-27 18:09_

---

_Closed by @AlexWaygood on 2025-05-27 18:09_

---

_Branch deleted on 2025-05-27 18:09_

---

_Comment by @github-actions[bot] on 2025-05-27 18:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---
