```yaml
number: 19636
title: "[ty] Remove special casing for tuple addition"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-add
created_at: 2025-07-30T12:06:52Z
updated_at: 2025-07-30T16:32:47Z
url: https://github.com/astral-sh/ruff/pull/19636
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Remove special casing for tuple addition

---

_Pull request opened by @AlexWaygood on 2025-07-30 12:06_

## Summary

This PR removes our special casing for tuple addition from `TypeInferenceBuilder::infer_binary_expression_type`. After #19635 (which this PR is based on top of) has landed, our generics solver works well enough with tuple types that we can just use the generic signature on the typeshed stub for `tuple.__add__` rather than implementing any special casing here for tuples. The typeshed `__add__` signature results in us inferring less precise types in many instances, but we nonetheless continue to infer a type that is _precise enough_ for nearly all practical use cases involving tuples. This can be seen from the fact that this PR has no impact on the typing conformance tests, and has a very minimal mypy_primer diff (two diagnostics removed, four diagnostics changed).

The special casing on `main` has minimal upside, then -- but it has significant downsides.
- It causes significant performance issues for various pathological cases involving tuples. This can be seen from the Codspeed results on this PR. We also no longer hang when checking the snippet given in https://github.com/astral-sh/ty/issues/765 -- in fact, we type-check it almost instantly. Even with this PR, it's probably possible to produce pathological code samples that will make ty's execution time blow up due to extremely large tuple types, and we should work on a more comprehensive fix for this issue. But I think it's notable that many of the cases we've found regarding this so far have been related to tuple addition, and this PR gets rid of that particular trigger for the problem.
- The special casing is currently not applied for tuple subclasses: but a subclass of `tuple[int, str]` should be treated exactly the same way as `tuple[int, str]` with respect to the special casing applied by ty, since we would allow an instance of the subclass to be passed wherever an instance of `tuple[int, str]` is expected. Extending the special casing so that it also applies to tuple subclasses would be possible, but in order to do this in a sound manner we would have to ban any overrides of `__add__` or `__radd__` on a tuple subclass. Otherwise, we'd have no way of ensuring that an `__(r)add__` override on a tuple subclass was sound, since the behaviour of `tuple.__(r)add__` cannot be described fully in a type annotation.

  Banning `__add__` and `__radd__` overrides on tuple subclasses feels overly restrictive to me; given the other downsides and the minimal upsides of this special casing, I lean towards simply removing it.

Closes https://github.com/astral-sh/ty/issues/765

## Test Plan

Mdtests updated


---

_Review requested from @carljm by @AlexWaygood on 2025-07-30 12:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-30 12:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-30 12:06_

---

_Label `ty` added by @AlexWaygood on 2025-07-30 12:06_

---

_Converted to draft by @AlexWaygood on 2025-07-30 12:06_

---

_Comment by @github-actions[bot] on 2025-07-30 12:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-30 12:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
vision (https://github.com/pytorch/vision)
- references/depth/stereo/transforms.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[()] | tuple[Unknown] | tuple[None], tuple[()] | tuple[Unknown] | tuple[None]]`
+ references/depth/stereo/transforms.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
- references/depth/stereo/transforms.py:485:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[()] | tuple[Unknown], tuple[()] | tuple[Unknown] | tuple[None], tuple[()] | tuple[Unknown] | tuple[None]]`
+ references/depth/stereo/transforms.py:485:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, ...], tuple[Unknown, ...] | tuple[None, ...], tuple[Unknown, ...] | tuple[None, ...]]`
- references/depth/stereo/transforms.py:579:9: error[invalid-assignment] Object of type `tuple[()] | tuple[Unknown] | tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]` is not assignable to `tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]`
+ references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...] | tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]` is not assignable to `tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]`
- references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[()] | tuple[None | Unknown] | tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]` is not assignable to `tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]`
- references/depth/stereo/transforms.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[(@Todo(Inference of subscript on special form) & None) | Unknown, (@Todo(Inference of subscript on special form) & None) | Unknown], tuple[()] | tuple[(@Todo(Inference of subscript on special form) & None) | Unknown]]`
+ references/depth/stereo/transforms.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)], tuple[@Todo(Inference of subscript on special form), @Todo(Inference of subscript on special form)]]`, found `tuple[tuple[Unknown, Unknown], tuple[(@Todo(Inference of subscript on special form) & None) | Unknown, (@Todo(Inference of subscript on special form) & None) | Unknown], tuple[(@Todo(Inference of subscript on special form) & None) | Unknown, ...]]`
- Found 1483 diagnostics
+ Found 1482 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/linalg/_decomp_schur.py:247:16: error[index-out-of-bounds] Index 0 is out of bounds for tuple `tuple[()]` with length 0
- Found 6803 diagnostics
+ Found 6802 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-30 12:16_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuple-add?runnerMode=Instrumentation)

### Merging #19636 will **improve performances by 90.81%**

<sub>Comparing <code>alex/tuple-add</code> (dbe53ab) with <code>main</code> (38049aa)</sub>



### Summary

`⚡ 1` improvements  
`✅ 40` untouched benchmarks  
`🆕 1` new benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[many_tuple_assignments] `` | 119.4 ms | 62.6 ms | +90.81% |
| 🆕 | `` ty_micro[many_tuple_assignments] `` | N/A | 57.9 ms | N/A |


---

_Marked ready for review by @AlexWaygood on 2025-07-30 12:33_

---

_@AlexWaygood reviewed on 2025-07-30 12:42_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty.rs`:357 on 2025-07-30 12:42_

```suggestion
    criterion.bench_function("ty_micro[tuple_implicit_instance_attributes]", |b| {
```

---

_Comment by @github-actions[bot] on 2025-07-30 12:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dcreager approved on 2025-07-30 14:17_

> The special casing on `main` has minimal upside, then

I do think it was a fun "look what we can do" feature, but that's not worth it given the downsides you've listed

---

_Merged by @AlexWaygood on 2025-07-30 16:25_

---

_Closed by @AlexWaygood on 2025-07-30 16:25_

---

_Branch deleted on 2025-07-30 16:25_

---
