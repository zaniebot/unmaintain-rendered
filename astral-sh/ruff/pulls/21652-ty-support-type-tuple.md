```yaml
number: 21652
title: "[ty] support `type[tuple[...]]`"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/typetuple
created_at: 2025-11-27T01:34:43Z
updated_at: 2025-12-01T17:11:32Z
url: https://github.com/astral-sh/ruff/pull/21652
synced_at: 2026-01-10T16:48:02Z
```

# [ty] support `type[tuple[...]]`

---

_Pull request opened by @carljm on 2025-11-27 01:34_

Fixes https://github.com/astral-sh/ty/issues/1649

## Summary

We missed this when adding support for `type[]` of a specialized generic.

## Test Plan

Added mdtests.


---

_Label `ty` added by @carljm on 2025-11-27 01:34_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 01:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 01:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/collections/__init__.pyi:43:22: error[invalid-type-arguments] Too many type arguments to class `tuple`: expected 1, got 2
- Found 1781 diagnostics
+ Found 1780 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 45 diagnostics
+ Found 44 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- scipy-stubs/_lib/_bunch.pyi:6:22: error[invalid-type-arguments] Too many type arguments to class `tuple`: expected 1, got 2
- Found 1972 diagnostics
+ Found 1971 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @carljm on 2025-11-27 01:43_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-27 01:43_

---

_Review requested from @sharkdp by @carljm on 2025-11-27 01:43_

---

_Review requested from @dcreager by @carljm on 2025-11-27 01:43_

---

_@ibraheemdev approved on 2025-11-27 01:47_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 01:51_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @ibraheemdev on 2025-11-27 01:53_

Was just about to comment about the typing conformance changes being identical to https://github.com/astral-sh/ruff/pull/21652.. but then I realized this PR was stacked on that one :smile: 

---

_@AlexWaygood reviewed on 2025-11-27 13:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1283 on 2025-11-27 13:45_

nit: since we have the custom `strum(serialize = "namedtuple")` attribute here anyway, could we call this variant `CollectionsNamedTuple`, to disambiguate it from `SpecialFormType::NamedTuple`?

---

_@AlexWaygood reviewed on 2025-11-27 13:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:11248 on 2025-11-27 13:50_

```suggestion
                let unknowns =
                    vec![Some(Type::unknown()); generic_context.variables(self.db()).len()];
```

---

_Comment by @AlexWaygood on 2025-11-27 13:52_

oops, I think I just accidentally left some review comments regarding changes that already landed in https://github.com/astral-sh/ruff/commit/77f8fa6906058b935eca7ae78c29322431abb377 (I guess this needs to be rebased now that's landed 😄)

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-01 09:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 09:52_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-arguments` | 0 | 2 | 0 |
| `unsupported-base` | 2 | 0 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **3** | **2** | **0** |

**[Full report with detailed diff](https://cjm-typetuple.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-typetuple.ecosystem-663.pages.dev/timing))




---

_Closed by @sharkdp on 2025-12-01 10:19_

---

_Reopened by @sharkdp on 2025-12-01 10:19_

---

_Comment by @sharkdp on 2025-12-01 10:19_

Re-running the ecosystem checks to pick up the changes in #21721.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-01 10:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-01 10:39_

---

_Merged by @sharkdp on 2025-12-01 10:49_

---

_Closed by @sharkdp on 2025-12-01 10:49_

---

_Branch deleted on 2025-12-01 10:49_

---

_Comment by @carljm on 2025-12-01 17:11_

Thanks for updating and landing this and the previous PR in stack @sharkdp !

---
