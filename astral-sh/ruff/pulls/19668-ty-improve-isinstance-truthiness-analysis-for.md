```yaml
number: 19668
title: "[ty] Improve `isinstance()` truthiness analysis for generic types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/isinstance-truthiness-3
created_at: 2025-07-31T18:47:54Z
updated_at: 2025-08-01T13:44:24Z
url: https://github.com/astral-sh/ruff/pull/19668
synced_at: 2026-01-12T15:56:44Z
```

# [ty] Improve `isinstance()` truthiness analysis for generic types

---

_@AlexWaygood_

## Summary

This PR extends the `isinstance()` truthiness inference (added in #19503) so that it works better for instances of generic types. Specifically, we now infer `Literal[True]` here, whereas on `main` we infer `bool`:

```py
def f(x: list[int]):
    reveal_type(isinstance(x, list))
```

The motivation for improving our logic here is that the logic on `main` breaks for tuples if we remove the `Type::Tuple` variant and treat them simply as instances of a (very special) generic class. I.e., we have tests asserting that we infer `isinstance((1, 2), tuple)` as `Literal[True]`; those break on the branch for #19669 without this change.

I've split this out into a separate PR from #19669 as it seems like a useful change on its own merits, however. We can see from the ecosystem report that it gets rid of two false positives from `bokeh`: we now correctly infer two branches of code as unreachable, and therefore do not emit any `invalid-return-type` diagnostics on those branches anymore. (See https://github.com/bokeh/bokeh/blob/adef0157284696ce86961b2089c75fddda53c15c/src/bokeh/core/property/container.py#L130-L140.)

I can't quite figure out what's going on in the sympy ecosystem hit (https://github.com/sympy/sympy/blob/3c817ed8ab551d71e36907ce869afe5e4ca789ff/sympy/polys/polyclasses.py#L308-L317) -- it seems like we used to infer the type of `g` as `DMP[Es] & DMP[Unknown]` at the point of the `return` statement, whereas we now infer the type of `g` as `DMP[Es]`. I don't quite understand why we now get to that answer; but still, our new inference seems better here -- intersecting with `DMP[Unknown]` as a result of the `isinstance()` check in that function is incorrect (https://github.com/astral-sh/ty/issues/456).

## Test Plan

I added mdtests for the direct functionality being added, and for the false positives that this PR removes from `bokeh`. I also extended the reachability tests added in #19503 to include some examples of generic types. Most of these tests currently fail, so there are many TODO comments added -- they require https://github.com/astral-sh/ty/issues/456 for them to be fixed. If this is merged, I'll add a comment to that issue saying that we now have some failing tests on `main` that should naturally be fixed if and when that issue is fixed.


---

_Comment by @github-actions[bot] on 2025-07-31 18:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @AlexWaygood on 2025-07-31 18:51_

---

_Comment by @github-actions[bot] on 2025-07-31 18:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/property/container.py:140:20: error[invalid-return-type] Return type does not match returned value: expected `PropertyValueList[T]`, found `list[T] & ~list[Unknown]`
- src/bokeh/core/property/container.py:165:20: error[invalid-return-type] Return type does not match returned value: expected `PropertyValueSet[T]`, found `set[T] & ~set[Unknown]`
- Found 836 diagnostics
+ Found 834 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/polys/polyclasses.py:314:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 12938 diagnostics
+ Found 12937 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] Improve `isinstance()` reachability analysis for generic types" to "[ty] Improve `isinstance()` truthiness analysis for generic types" by @AlexWaygood on 2025-08-01 13:25_

---

_Marked ready for review by @AlexWaygood on 2025-08-01 13:25_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-01 13:25_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-01 13:25_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-01 13:25_

---

_@sharkdp approved on 2025-08-01 13:39_

Looks good — Thank you!

---

_Merged by @AlexWaygood on 2025-08-01 13:44_

---

_Closed by @AlexWaygood on 2025-08-01 13:44_

---

_Branch deleted on 2025-08-01 13:44_

---
