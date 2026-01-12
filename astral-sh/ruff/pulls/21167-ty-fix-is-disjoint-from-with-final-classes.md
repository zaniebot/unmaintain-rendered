```yaml
number: 21167
title: "[ty] Fix `is_disjoint_from` with `@final` classes"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
assignees: []
merged: true
base: main
head: ibraheem/disjoint-final
created_at: 2025-10-31T14:32:14Z
updated_at: 2025-10-31T14:50:56Z
url: https://github.com/astral-sh/ruff/pull/21167
synced_at: 2026-01-12T15:57:17Z
```

# [ty] Fix `is_disjoint_from` with `@final` classes

---

_@ibraheemdev_

## Summary

We currently perform a subtyping check instead of the intended subclass check (and the subtyping check is confusingly named `is_subclass_of`). This showed up in https://github.com/astral-sh/ruff/pull/21070.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-31 14:32_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-31 14:32_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-31 14:32_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-31 14:32_

---

_Label `ty` added by @ibraheemdev on 2025-10-31 14:32_

---

_Comment by @github-actions[bot] on 2025-10-31 14:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-31 14:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md`:112 on 2025-10-31 14:37_

`int` and `str` are disjoint bases, so these two actually _should_ be disjoint (we're missing a check somewhere else). So this would be a better test here:

```suggestion
class A: ...
class B: ...

static_assert(not is_disjoint_from(Foo[A], Foo[B]))
```

---

_@AlexWaygood approved on 2025-10-31 14:37_

Thanks!

---

_Merged by @ibraheemdev on 2025-10-31 14:50_

---

_Closed by @ibraheemdev on 2025-10-31 14:50_

---

_Branch deleted on 2025-10-31 14:50_

---
