```yaml
number: 18342
title: "[ty] Simplify `Type::try_bool()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-bool
created_at: 2025-05-27T18:26:18Z
updated_at: 2025-05-27T18:33:44Z
url: https://github.com/astral-sh/ruff/pull/18342
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Simplify `Type::try_bool()`

---

_@AlexWaygood_

## Summary

I don't think we're ever going to add any `KnownInstanceType` variants that evaluate to `False` in a boolean context; the `KnownInstanceType::bool()` method just seems like unnecessary complexity.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-27 18:26_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-27 18:26_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-27 18:26_

---

_Label `internal` added by @AlexWaygood on 2025-05-27 18:26_

---

_Label `ty` added by @AlexWaygood on 2025-05-27 18:26_

---

_Comment by @github-actions[bot] on 2025-05-27 18:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-27 18:31_

---

_Comment by @carljm on 2025-05-27 18:31_

Never thought I'd see the day that @AlexWaygood would remove an exhaustive match and replace it with a `_` ;)

---

_Merged by @AlexWaygood on 2025-05-27 18:32_

---

_Closed by @AlexWaygood on 2025-05-27 18:32_

---

_Branch deleted on 2025-05-27 18:32_

---

_Comment by @AlexWaygood on 2025-05-27 18:33_

> Never thought I'd see the day that @AlexWaygood would remove an exhaustive match and replace it with a `_` ;)

he comes back from PyCon and all of a sudden he's dreaming of beautiful, concise Python code...

---
