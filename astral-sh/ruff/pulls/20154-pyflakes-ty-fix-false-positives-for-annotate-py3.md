```yaml
number: 20154
title: "[`pyflakes`] [ty] Fix false positives for `__annotate__` (Py3.14+) and `__warningregistry__` (`F821`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
  - ty
assignees: []
merged: true
base: main
head: fix-19970
created_at: 2025-08-29T16:40:35Z
updated_at: 2025-09-23T12:40:54Z
url: https://github.com/astral-sh/ruff/pull/20154
synced_at: 2026-01-12T15:56:55Z
```

# [`pyflakes`] [ty] Fix false positives for `__annotate__` (Py3.14+) and `__warningregistry__` (`F821`)

---

_@danparizher_

## Summary

Fixes #19970


---

_Review requested from @carljm by @danparizher on 2025-08-29 16:40_

---

_Review requested from @AlexWaygood by @danparizher on 2025-08-29 16:40_

---

_Review requested from @sharkdp by @danparizher on 2025-08-29 16:40_

---

_Review requested from @dcreager by @danparizher on 2025-08-29 16:40_

---

_Comment by @github-actions[bot] on 2025-08-29 16:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 16:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
CPython (cases_generator) (https://github.com/python/cpython)
- Lib/test/test_warnings/__init__.py:38:12: warning[unresolved-global] Invalid global declaration of `__warningregistry__`: `__warningregistry__` has no declarations or bindings in the global scope
- Lib/test/test_warnings/__init__.py:45:9: error[unresolved-reference] Name `__warningregistry__` used when not defined
+ Lib/test/test_warnings/__init__.py:45:9: warning[possibly-unresolved-reference] Name `__warningregistry__` used when possibly not defined
- Lib/test/test_warnings/__init__.py:153:35: error[unresolved-reference] Name `__warningregistry__` used when not defined
+ Lib/test/test_warnings/__init__.py:153:35: warning[possibly-unresolved-reference] Name `__warningregistry__` used when possibly not defined
- Lib/test/test_warnings/__init__.py:844:16: warning[unresolved-global] Invalid global declaration of `__warningregistry__`: `__warningregistry__` has no declarations or bindings in the global scope
- Found 23904 diagnostics
+ Found 23902 diagnostics

CPython (Argument Clinic) (https://github.com/python/cpython)
- Lib/test/test_warnings/__init__.py:38:12: warning[unresolved-global] Invalid global declaration of `__warningregistry__`: `__warningregistry__` has no declarations or bindings in the global scope
- Lib/test/test_warnings/__init__.py:45:9: error[unresolved-reference] Name `__warningregistry__` used when not defined
+ Lib/test/test_warnings/__init__.py:45:9: warning[possibly-unresolved-reference] Name `__warningregistry__` used when possibly not defined
- Lib/test/test_warnings/__init__.py:153:35: error[unresolved-reference] Name `__warningregistry__` used when not defined
+ Lib/test/test_warnings/__init__.py:153:35: warning[possibly-unresolved-reference] Name `__warningregistry__` used when possibly not defined
- Lib/test/test_warnings/__init__.py:844:16: warning[unresolved-global] Invalid global declaration of `__warningregistry__`: `__warningregistry__` has no declarations or bindings in the global scope
- Found 23904 diagnostics
+ Found 23902 diagnostics

CPython (peg_generator) (https://github.com/python/cpython)
- Lib/test/test_warnings/__init__.py:38:12: warning[unresolved-global] Invalid global declaration of `__warningregistry__`: `__warningregistry__` has no declarations or bindings in the global scope
- Lib/test/test_warnings/__init__.py:45:9: error[unresolved-reference] Name `__warningregistry__` used when not defined
+ Lib/test/test_warnings/__init__.py:45:9: warning[possibly-unresolved-reference] Name `__warningregistry__` used when possibly not defined
- Lib/test/test_warnings/__init__.py:153:35: error[unresolved-reference] Name `__warningregistry__` used when not defined
+ Lib/test/test_warnings/__init__.py:153:35: warning[possibly-unresolved-reference] Name `__warningregistry__` used when possibly not defined
- Lib/test/test_warnings/__init__.py:844:16: warning[unresolved-global] Invalid global declaration of `__warningregistry__`: `__warningregistry__` has no declarations or bindings in the global scope
- Found 23903 diagnostics
+ Found 23901 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-29 16:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ty_python_semantic/src/place.rs`:1 on 2025-09-09 16:28_

This change looks reasonable to me, but we'll want someone from the ty side to have a look too, maybe @AlexWaygood? I'm guessing they'd like you to have an mdtest for this as well. Possibly in [`moduletype_attrs.md`](https://github.com/astral-sh/ruff/blob/c3f1f0b9f1ed8e522a6d22c7d1670997c19183de/crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md?plain=1#L1), but I'm not really sure.

---

_Review comment by @ntBre on `crates/ruff_python_stdlib/src/builtins.rs`:23 on 2025-09-09 16:35_

I think it's correct to put `__warningregistry__` here because I don't think it's available in `builtins`, but I think we should create a separate `PY314_PLUS_BUILTINS` down below like the other version-specific groups for `__annotate__` since it was introduced in 3.14.

It appears not to _really_ matter since both `MAGIC_GLOBALS` and `python_builtins` are only called once each in the same place. But we may also need to update `is_python_builtin` since its comment says it's supposed to be kept in sync with `python_builtins`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:934 on 2025-09-09 16:37_

This doesn't seem to be testing what the test name suggests. As the comment says, the `flakes` test runner uses the `latest` version, which is still 3.13, so we'll need to write some custom test code here. We've been talking about switching this whole module over to the more common `test_path` and `assert_diagnostics` tests like most other rules, so I'd probably suggest just doing something like that here. One example of that:

https://github.com/astral-sh/ruff/blob/c3f1f0b9f1ed8e522a6d22c7d1670997c19183de/crates/ruff_linter/src/rules/ruff/mod.rs#L116-L125

We should also test that `__annotate__` is not available before 3.14. I think this test should actually be failing given the version being used.

---

_@ntBre reviewed on 2025-09-09 16:53_

---

_Review request for @carljm removed by @carljm on 2025-09-09 16:54_

---

_Comment by @danparizher on 2025-09-09 19:07_

Thanks for the detailed review!

I went ahead and version-gated magic globals by introducing `python_magic_globals` (with `__annotate__` behind `PY314_PLUS_MAGIC_GLOBALS`) and updated the linter to bind them per target version; I left `is_python_builtin` as-is since `__annotate__` isn’t a builtin.

I also reworked the pyflakes tests to explicitly check `__annotate__` on 3.13 vs 3.14, and added/adjusted ty mdtests to model `__warningregistry__` as possibly unbound and to validate the `__annotate__` gating. All tests pass locally.

---

_@ntBre reviewed on 2025-09-19 20:12_

Thanks! This looks good to me on the Ruff side. 

---

_Comment by @MichaReiser on 2025-09-23 11:18_

@AlexWaygood would you mind taking a look at how the changes impact ty?

---

_@AlexWaygood approved on 2025-09-23 11:54_

Thanks, this looks great! I made a couple of small tweaks to the ty changes so that we infer some more precise types for these symbols. I also made `__annotate__` possibly-unbound as well as `__warningregistry__`, since `__annotate__` is only present in the global scope if a module has an annotation like `x: int` or `x: int = 42` in the global scope

---

_Comment by @AlexWaygood on 2025-09-23 12:11_

The primer report LGTM too!

---

_Review requested from @ntBre by @AlexWaygood on 2025-09-23 12:11_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-09-23 12:12_

---

_Review request for @dcreager removed by @AlexWaygood on 2025-09-23 12:12_

---

_@ntBre approved on 2025-09-23 12:14_

Thank you both!

---

_Label `bug` added by @ntBre on 2025-09-23 12:15_

---

_Label `rule` added by @ntBre on 2025-09-23 12:15_

---

_Merged by @ntBre on 2025-09-23 12:16_

---

_Closed by @ntBre on 2025-09-23 12:16_

---

_Label `ty` added by @AlexWaygood on 2025-09-23 12:19_

---

_Renamed from "[`pyflakes`] Fix false positives for `__annotate__` (Py3.14+) and `__warningregistry__` (`F821`)" to "[`pyflakes`] [ty] Fix false positives for `__annotate__` (Py3.14+) and `__warningregistry__` (`F821`)" by @AlexWaygood on 2025-09-23 12:19_

---

_Branch deleted on 2025-09-23 12:40_

---
