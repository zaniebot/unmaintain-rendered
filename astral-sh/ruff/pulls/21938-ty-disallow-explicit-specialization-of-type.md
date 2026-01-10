```yaml
number: 21938
title: "[ty] disallow explicit specialization of type variables themselves"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: ban-typevar-specialization
created_at: 2025-12-12T09:34:54Z
updated_at: 2025-12-15T08:42:51Z
url: https://github.com/astral-sh/ruff/pull/21938
synced_at: 2026-01-10T16:42:11Z
```

# [ty] disallow explicit specialization of type variables themselves

---

_Pull request opened by @mtshiba on 2025-12-12 09:34_

## Summary

This PR makes explicit specialization of a type variable itself an error, and the result of the specialization is `Unknown`.

The change also fixes https://github.com/astral-sh/ty/issues/1794.

## Test Plan

mdtests updated
new corpus test


---

_Comment by @astral-sh-bot[bot] on 2025-12-12 09:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 09:39_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @mtshiba on 2025-12-12 12:18_

---

_Review requested from @carljm by @mtshiba on 2025-12-12 12:18_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-12-12 12:18_

---

_Review requested from @sharkdp by @mtshiba on 2025-12-12 12:18_

---

_Review requested from @dcreager by @mtshiba on 2025-12-12 12:18_

---

_Review requested from @MichaReiser by @mtshiba on 2025-12-12 12:18_

---

_Label `ty` added by @AlexWaygood on 2025-12-12 12:21_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:118 on 2025-12-12 13:04_

This usage seems to be used in [bokeh](https://github.com/bokeh/bokeh/blob/branch-3.9/src/bokeh/_types.pyi#L19
), so we should allow it.

---

_@mtshiba reviewed on 2025-12-12 13:04_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:118 on 2025-12-12 21:55_

For reference, pyright and pyrefly support both `ImplicitPositive` and `Positive` forms. Mypy supports only the explicit PEP 613 version.

With our current implementation of implicit type aliases, it would be quite difficult for us to distinguish `ImplicitPositive` from direct use of `T`. This may change in the future, but for now it seems fine to match mypy's support.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:9484 on 2025-12-12 21:56_

This variant is unlike the others -- it is not really a kind of typevar, but rather a kind of type alias -- a direct alias to a typevar.

I think this is the most direct implementation, but I would probably name and document this variant so as to emphasize its differentness more.

---

_@carljm reviewed on 2025-12-12 21:56_

Looks good, thanks! A few nits, I will just make some minor updates and then merge.

---

_Merged by @carljm on 2025-12-12 23:49_

---

_Closed by @carljm on 2025-12-12 23:49_

---

_Branch deleted on 2025-12-15 08:42_

---
