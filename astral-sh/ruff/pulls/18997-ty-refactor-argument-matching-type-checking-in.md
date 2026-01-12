```yaml
number: 18997
title: "[ty] Refactor argument matching / type checking in call binding"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/refactor-call-binding
created_at: 2025-06-27T19:34:09Z
updated_at: 2025-06-27T21:01:55Z
url: https://github.com/astral-sh/ruff/pull/18997
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Refactor argument matching / type checking in call binding

---

_@dcreager_

This PR extracts a lot of the complex logic in the `match_parameters` and `check_types` methods of our call binding machinery into separate helper types. This is setup for #18996, which will update this logic to handle variadic arguments. To do so, it is helpful to have the per-argument logic extracted into a method that we can call repeatedly for each _element_ of a variadic argument.

This should be a pure refactoring, with no behavioral changes.

---

_Review requested from @carljm by @dcreager on 2025-06-27 19:34_

---

_Review requested from @AlexWaygood by @dcreager on 2025-06-27 19:34_

---

_Review requested from @sharkdp by @dcreager on 2025-06-27 19:34_

---

_Label `internal` added by @dcreager on 2025-06-27 19:34_

---

_Label `ty` added by @dcreager on 2025-06-27 19:34_

---

_Comment by @github-actions[bot] on 2025-06-27 19:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-06-27 20:27_

Looks good!

---

_Merged by @dcreager on 2025-06-27 21:01_

---

_Closed by @dcreager on 2025-06-27 21:01_

---

_Branch deleted on 2025-06-27 21:01_

---
