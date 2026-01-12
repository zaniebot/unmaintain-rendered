```yaml
number: 20324
title: "[ty] Simplify unions of enum literals and subtypes thereof"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-1155
created_at: 2025-09-10T07:41:17Z
updated_at: 2025-09-10T13:54:09Z
url: https://github.com/astral-sh/ruff/pull/20324
synced_at: 2026-01-12T15:56:59Z
```

# [ty] Simplify unions of enum literals and subtypes thereof

---

_@sharkdp_

## Summary

When adding an enum literal `E = Literal[Color.RED]` to a union which already contained a subtype of that enum literal(!), we were previously not simplifying the union correctly. My assumption is that our property tests didn't catch that earlier, because the only possible non-trivial subytpe of an enum literal that I can think of is `Any & E`. And in order for that to be detected by the property tests, it would have to randomly generate `Any & E | E` and then also compare that with `E` on the other side (in an equivalence test, or the subtyping-antisymmetry test).

closes https://github.com/astral-sh/ty/issues/1155

## Test Plan

* Added a regression test.
* I also ran the property tests for a while, but probably not for two months worth of daily CI runs.


---

_Label `ty` added by @sharkdp on 2025-09-10 07:41_

---

_Review requested from @carljm by @sharkdp on 2025-09-10 07:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-10 07:41_

---

_Review requested from @dcreager by @sharkdp on 2025-09-10 07:41_

---

_@sharkdp reviewed on 2025-09-10 07:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/builder.rs`:448 on 2025-09-10 07:42_

This was the problem. Instead of just using `self.elements.push`, we should go through the whole simplification logic in the `_ => {…}` branch, which I have now moved to a separate function. Every other change in this file is just the "moving out to a separate function" part.

---

_Comment by @github-actions[bot] on 2025-09-10 07:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 07:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-09-10 13:53_

---

_Merged by @sharkdp on 2025-09-10 13:54_

---

_Closed by @sharkdp on 2025-09-10 13:54_

---

_Branch deleted on 2025-09-10 13:54_

---
