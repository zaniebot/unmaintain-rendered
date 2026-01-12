```yaml
number: 21165
title: "[ty] Prefer exact matches when solving constrained type variables"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/constrained-typevar-solving
created_at: 2025-10-31T14:27:47Z
updated_at: 2025-10-31T14:58:11Z
url: https://github.com/astral-sh/ruff/pull/21165
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Prefer exact matches when solving constrained type variables

---

_@ibraheemdev_

## Summary

The solver is currently order-dependent, and will choose a supertype over the exact type if it appears earlier in the list of constraints. We could be smarter and try to choose the most precise subtype, but I imagine this is something the new constraint solver will fix anyways, and this fixes the issue showing up on https://github.com/astral-sh/ruff/pull/21070.


---

_Review requested from @carljm by @ibraheemdev on 2025-10-31 14:27_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-31 14:27_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-31 14:27_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-31 14:27_

---

_Label `ty` added by @ibraheemdev on 2025-10-31 14:27_

---

_Comment by @github-actions[bot] on 2025-10-31 14:29_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 14:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
meson (https://github.com/mesonbuild/meson)
- mesonbuild/cargo/builder.py:63:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Token[bool]`, found `Token[int]`
- Found 1655 diagnostics
+ Found 1654 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@dcreager approved on 2025-10-31 14:52_

Looks good! And this will be a nice regression test for when we start to land the new solver

---

_Merged by @ibraheemdev on 2025-10-31 14:58_

---

_Closed by @ibraheemdev on 2025-10-31 14:58_

---

_Branch deleted on 2025-10-31 14:58_

---
