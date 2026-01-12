```yaml
number: 20860
title: "[ty] Treat `Callable` dunder members as bound method descriptors"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/treat-dunder-method-callables-as-bound-method-descriptors
created_at: 2025-10-14T10:12:11Z
updated_at: 2025-10-14T12:27:54Z
url: https://github.com/astral-sh/ruff/pull/20860
synced_at: 2026-01-12T15:57:11Z
```

# [ty] Treat `Callable` dunder members as bound method descriptors

---

_@sharkdp_

## Summary

Dunder methods (at least the ones defined in the standard library) always take an instance of the class as the first parameter. So it seems reasonable to generally treat them as bound method descriptors if they are defined via a `Callable` type.

This removes just a few false positives from the ecosystem, but solves three user-reported issues:

closes https://github.com/astral-sh/ty/issues/908
closes https://github.com/astral-sh/ty/issues/1143
closes https://github.com/astral-sh/ty/issues/1209

In addition to the change here, I also considered [making `ClassVar`s bound method descriptors](https://github.com/astral-sh/ruff/pull/20861). However, there was zero ecosystem impact. So I think we can also close https://github.com/astral-sh/ty/issues/491 with this PR.

closes https://github.com/astral-sh/ty/issues/491

## Test Plan

Added regression test


---

_Label `ty` added by @sharkdp on 2025-10-14 10:12_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-14 10:12_

---

_Comment by @github-actions[bot] on 2025-10-14 10:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 10:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/dataclasses.py:315:17: error[missing-argument] No argument provided for required parameter 1
- pydantic/v1/dataclasses.py:332:17: error[missing-argument] No argument provided for required parameter 1
- Found 767 diagnostics
+ Found 765 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/testtools.py:1931:41: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`
- hydpy/cythons/modelutils.py:2110:44: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__code__`
- Found 611 diagnostics
+ Found 609 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-14 10:17_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `missing-argument` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| **Total** | **0** | **4** | **0** |

**[Full report with detailed diff](https://david-treat-dunder-method-ca.ecosystem-663.pages.dev/diff)** ([timing results](https://david-treat-dunder-method-ca.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-14 10:59_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-14 10:59_

---

_@sharkdp reviewed on 2025-10-14 11:41_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:1195 on 2025-10-14 11:41_

The previous example here would have failed at runtime.

---

_Marked ready for review by @sharkdp on 2025-10-14 11:48_

---

_Review requested from @carljm by @sharkdp on 2025-10-14 11:48_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-14 11:48_

---

_Review requested from @dcreager by @sharkdp on 2025-10-14 11:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2026 on 2025-10-14 12:16_

of course, if we just represented function-like callables as `FunctionType & <some Callable type>`, all we'd need to do here would be to insert `FunctionType` into the intersection; no need for a new `Intersection::map_positive` method!

...anyway, sorry for beating this drum, I'll try to shut up about beautiful refactors we could do until the beta's out 🙈

---

_@AlexWaygood approved on 2025-10-14 12:16_

---

_@sharkdp reviewed on 2025-10-14 12:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2026 on 2025-10-14 12:27_

If things wouldn't always turn out to be more complicated than I initially think, I would have already started that project. But I'm almost certain this would reveal at least three issues with our intersection handling, lead to unexpected performance regressions, and probably uncover a new salsa concurrency bug. So it has to wait a little longer :smile:.

---

_Merged by @sharkdp on 2025-10-14 12:27_

---

_Closed by @sharkdp on 2025-10-14 12:27_

---

_Branch deleted on 2025-10-14 12:27_

---
