```yaml
number: 20142
title: "[ty] Enforce that an attribute on a class `X` must be callable in order to satisfy a member on a protocol `P`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/dumb-method-members
created_at: 2025-08-28T19:55:50Z
updated_at: 2025-08-29T07:31:28Z
url: https://github.com/astral-sh/ruff/pull/20142
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Enforce that an attribute on a class `X` must be callable in order to satisfy a member on a protocol `P`

---

_Pull request opened by @AlexWaygood on 2025-08-28 19:55_

## Summary

Small, incremental progress towards checking the types of method members.

## Test Plan

Added an mdtest


---

_Label `ty` added by @AlexWaygood on 2025-08-28 19:55_

---

_Comment by @github-actions[bot] on 2025-08-28 19:57_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 19:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/data/btanalysis/bt_fileutils.py:377:22: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `list[@Todo(list literal element type)]`
+ freqtrade/freqai/RL/BaseReinforcementLearningModel.py:371:13: error[no-matching-overload] No overload of bound method `drop` matches arguments
+ freqtrade/optimize/backtesting.py:479:32: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `Unknown | list[@Todo(list literal element type)]`
- Found 384 diagnostics
+ Found 387 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-28 20:26_

> ```diff
> freqtrade (https://github.com/freqtrade/freqtrade)
> + freqtrade/data/btanalysis/bt_fileutils.py:377:22: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `list[@Todo(list literal element type)]`
> + freqtrade/freqai/RL/BaseReinforcementLearningModel.py:371:13: error[no-matching-overload] No overload of bound method `drop` matches arguments
> + freqtrade/optimize/backtesting.py:479:32: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `Unknown | list[@Todo(list literal element type)]`
> - Found 384 diagnostics
> + Found 387 diagnostics
> ```

For these diagnostics, it looks like our eager simplification of unions is coming up against the fact that `Hashable` fundamentally doesn't make sense as a type in Python. (I wish pandas-stubs wouldn't use it quite so much!)

The relevant overload for `pandas.DataFrame.drop()` (inherited from its base class `NDFrame`) is here: https://github.com/pandas-dev/pandas-stubs/blob/e959fc933b024f7af6ad6efb5e8f07e8772ede61/pandas-stubs/core/generic.pyi#L341-L352. We can see that it is annotated with `Hashable | Sequence[Hashable] | Index[Any]`, which I think ty then immediately simplifies to `Hashable | Index[Any]`, since `Sequence[Hashable]` is a subtype of `Hashable`. The only trouble is, `list[Unknown]` and `list[str]` are both assignable to `Sequence[Hashable]` but not to `Hashable` (because `list` overrides `__hash__` with `None` in typeshed's stubs to indicate that it is not hashable). That then leads us to reject this call [here](https://github.com/freqtrade/freqtrade/blob/1cf1d9e3d7af4c18b09e2d6e83b4da5eadfe004c/freqtrade/data/btanalysis/bt_fileutils.py#L377), even though it's clearly meant to be allowed.

I guess to fix this we could either simplify unions less or special-case `Hashable` in some way? Not sure.

---

_Renamed from "[ty] Small incremental progress towards checking the types of method members on protocols" to "[ty] Enforce that an attribute on a class `X` must be callable in order to satisfy a member on a protocol `P`" by @AlexWaygood on 2025-08-28 20:26_

---

_Marked ready for review by @AlexWaygood on 2025-08-28 20:28_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-28 20:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-28 20:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-28 20:28_

---

_@carljm approved on 2025-08-28 23:10_

Ugh, Hashable is really a problem :/

---

_Merged by @AlexWaygood on 2025-08-29 07:31_

---

_Closed by @AlexWaygood on 2025-08-29 07:31_

---

_Branch deleted on 2025-08-29 07:31_

---
