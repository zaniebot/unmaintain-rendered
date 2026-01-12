```yaml
number: 20140
title: "[ty] Add more specific diagnostics for unsupported comparison diagnostics"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
  - diagnostics
assignees: []
base: main
head: unsupported-operator-diagnostics
created_at: 2025-08-28T18:46:27Z
updated_at: 2026-01-09T02:09:00Z
url: https://github.com/astral-sh/ruff/pull/20140
synced_at: 2026-01-12T15:56:55Z
```

# [ty] Add more specific diagnostics for unsupported comparison diagnostics

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/921

We now emit an extra info diagnostic explaining why the rich comparison is not supported.

## Test Plan

Add tests to `crates/ty_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`


---

_Comment by @github-actions[bot] on 2025-08-28 18:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 19:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy (https://github.com/python/mypy)
+ mypy/reachability.py:274:21: error[unsupported-operator] Operator `==` is not supported for types `Targ@fixed_comparison` and `Targ@fixed_comparison`
+ mypy/reachability.py:276:21: error[unsupported-operator] Operator `!=` is not supported for types `Targ@fixed_comparison` and `Targ@fixed_comparison`
- Found 1846 diagnostics
+ Found 1848 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/externals/array_api_extra/_lib/_utils/_helpers.py:93:25: error[unsupported-operator] Operator `!=` is not supported for types `Array` and `object`
+ sklearn/externals/array_api_extra/_lib/_utils/_helpers.py:97:25: error[unsupported-operator] Operator `==` is not supported for types `Array` and `object`
- sklearn/neighbors/_base.py:950:13: error[non-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- Found 1992 diagnostics
+ Found 1993 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
+ python/tests/test_high_level.py:1261:16: error[unsupported-operator] Operator `==` is not supported for types `BaseExpr` and `<class 'E'>`, in comparing `((...) -> BaseExpr) | BaseExpr | None` with `<class 'E'>`
- Found 1369 diagnostics
+ Found 1370 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/unit/test_index.py:520:26: error[unresolved-attribute] Object of type `bool` has no attribute `tolist`
+ static_frame/test/unit/test_index.py:520:27: error[unsupported-operator] Operator `==` is not supported for types `Index[Any]` and `Index[Any]`

scipy (https://github.com/scipy/scipy)
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:93:25: error[unsupported-operator] Operator `!=` is not supported for types `Array` and `object`
+ scipy/_lib/array_api_extra/src/array_api_extra/_lib/_utils/_helpers.py:97:25: error[unsupported-operator] Operator `==` is not supported for types `Array` and `object`
- Found 6951 diagnostics
+ Found 6953 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-08-28 19:17_

in sympy The type of `Poly().__eq__` is `(Unknown, Unknown, /) -> Unknown` (due to the use of a decorator which changes the signature of the instance method). So that is why we get those primer hits.

This is quite unfortunate

---

_Comment by @MatthewMckee4 on 2025-08-28 19:20_

I'm unsure about the other mypy primer hits here

---

_Label `ty` added by @AlexWaygood on 2025-08-28 19:20_

---

_Label `diagnostics` added by @AlexWaygood on 2025-08-28 19:20_

---

_Renamed from "Add more specific diagnostics for unsupported comparison diagnostics" to "[ty] Add more specific diagnostics for unsupported comparison diagnostics" by @MatthewMckee4 on 2025-08-28 19:20_

---

_Marked ready for review by @MatthewMckee4 on 2025-08-28 19:21_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-28 19:21_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-28 19:21_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-28 19:21_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-28 19:21_

---

_Comment by @carljm on 2025-08-29 00:11_

Thanks for the PR! 

Regarding the sympy diagnostics: an annotation of `Unknown` should never result in an error, if there is any possible annotation it could be replaced with that would not be an error. I haven't looked at the PR code changes in detail yet, but if we are emitting lots of new errors due to this signature full of `Unknown`, that suggests this PR is doing something incorrect (maybe using a subtype check where it should be using an assignability check? Not sure)

I think it's clear that we can't land this without fixing that -- let me know if you need help figuring out the cause

---

_Comment by @MatthewMckee4 on 2025-08-29 12:42_

The problem I was trying to show was that the type was `(Unknown, Unknown, /) -> Unknown` and not `(Unknown, /) -> Unknown`

---

_Comment by @carljm on 2025-08-29 16:25_

Oh, sorry, I missed that! That makes sense. I think it's a case of https://github.com/astral-sh/ty/issues/491 then. The fact that this comes up with decorated methods where the decorator is annotated to return a `Callable` does increase the priority on doing something different there.

In that case maybe we can land this with those new diagnostics -- the same issue already affects any normal method, this just makes it show up in a few more cases with dunder comparison methods.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:453 on 2025-08-29 16:26_

This appears to be an exact copy of the previous test, it doesn't seem to show "possibly not callable"

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:7990 on 2025-08-29 16:37_

I think when we are talking about something (possibly) not being callable or bound, then we need to be speaking in terms of the dunder method, not the operator. It doesn't make sense to say "operator '==' is not callable", it makes sense to say "method `__eq__` is not callable"

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8556 on 2025-08-29 16:42_

This change looks a bit suspicious -- it seems like we now ignore the passed-in `policy`, except that we consider it below in some code that now looks wrong (since we are now always using "no-object-fallback" policy here).

I think we need to determine whether this method needs to accept a passed-in policy (in which case we should ensure that policy is correct at the call sites and we should not override it here) or not (in which case we should just remove the argument).

---

_@carljm requested changes on 2025-08-29 16:42_

This looks really good, thank you! A couple requested changes.

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-01 07:48_

---

_@MatthewMckee4 reviewed on 2025-09-06 00:16_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/resources/mdtest/comparison/instances/rich_comparison.md`:453 on 2025-09-06 00:16_

I have removed this now

---

_@MatthewMckee4 reviewed on 2025-09-06 00:17_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/infer.rs`:7990 on 2025-09-06 00:17_

yeah makes sense, I have updated the diagnostic now

---

_Comment by @github-actions[bot] on 2025-09-06 00:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @carljm by @MatthewMckee4 on 2025-10-23 14:05_

---

_Comment by @carljm on 2025-11-06 16:20_

It looks like this is now panicking on an ecosystem project?

---

_Comment by @MatthewMckee4 on 2025-11-06 17:36_

There are a few places that call expect, expecting eq to never error, I would suspect that to bean issue, on things like 'in' checks.

Seems I need to update some of that code.

I can't check the actual error for a few days

---

_Comment by @MatthewMckee4 on 2025-11-06 17:38_

Yeah, that's exactly what it is
```
fatal[panic] Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:9495:18 when checking `/tmp/mypy_primer/projects/operator/ops/framework.py`: `infer_binary_type_comparison should never return None for `CmpOp::Eq`: CompareUnsupportedError { op: Eq, left_ty: NominalInstance(NominalInstanceType(NonTuple(NonGeneric(ClassLiteral { name: Name("Handle"), body_scope: ScopeId { [salsa id]: Id(14407), file: File(System("/tmp/mypy_primer/projects/operator/ops/framework.py")), file_scope_id: FileScopeId(7) }, known: None, deprecated: None, type_check_only: false, dataclass_params: None, dataclass_transformer_params: None })))), right_ty: NominalInstance(NominalInstanceType(NonTuple(NonGeneric(ClassLiteral { name: Name("NoneType"), body_scope: ScopeId { [salsa id]: Id(21148), file: File(Vendored(VendoredPathBuf("stdlib/types.pyi"))), file_scope_id: FileScopeId(222) }, known: Some(NoneType), deprecated: None, type_check_only: false, dataclass_params: None, dataclass_transformer_params: None })))), reason: Some("Operator '==' from dunder method `__eq__` on object of type 'Handle' cannot be used with object of type 'None'") }`

```

---

_Comment by @carljm on 2026-01-09 02:09_

Closing this to clean up the PR queue since it's quite old -- feel free to reopen if you resurrect it!

---

_Closed by @carljm on 2026-01-09 02:09_

---
