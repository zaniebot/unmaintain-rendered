```yaml
number: 17958
title: "[ty] Remove `SliceLiteral` type variant"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/remove-slice
created_at: 2025-05-08T16:26:46Z
updated_at: 2025-05-09T00:16:43Z
url: https://github.com/astral-sh/ruff/pull/17958
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Remove `SliceLiteral` type variant

---

_@dcreager_

@AlexWaygood pointed out that the `SliceLiteral` type variant was originally created to handle slices before we had generics. https://github.com/astral-sh/ruff/pull/17927#discussion_r2078115787

Now that we _do_ have generics, we can use a specialization of the `slice` builtin type for slice literals.

This depends on https://github.com/astral-sh/ruff/pull/17956, since we need to make sure that all typevar defaults are fully substituted when specializing `slice`.

---

_Review requested from @carljm by @dcreager on 2025-05-08 16:26_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-08 16:26_

---

_Review requested from @sharkdp by @dcreager on 2025-05-08 16:26_

---

_Label `ty` added by @dcreager on 2025-05-08 16:26_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2679 on 2025-05-08 18:36_

Isn't there an additional qualification here? It has to represent a valid specialization of `slice`, where the specializations are types that allow us to perform slicing with statically-known numeric values (i.e. integer or boolean literals, or `None`) -- otherwise we'll just return `None`

---

_@carljm approved on 2025-05-08 18:37_

Love this so much.

---

_@dcreager reviewed on 2025-05-08 22:46_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2679 on 2025-05-08 22:46_

Ha yeah I was maybe making "valid" too load bearing there....  :sweat_smile: 

Done

---

_Comment by @github-actions[bot] on 2025-05-08 22:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-08 23:10_

---

_Merged by @dcreager on 2025-05-09 00:16_

---

_Closed by @dcreager on 2025-05-09 00:16_

---

_Branch deleted on 2025-05-09 00:16_

---
