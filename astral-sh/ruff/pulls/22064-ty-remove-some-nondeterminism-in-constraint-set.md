```yaml
number: 22064
title: "[ty] Remove some nondeterminism in constraint set tests"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/nondeterminism
created_at: 2025-12-18T22:10:58Z
updated_at: 2025-12-19T00:00:22Z
url: https://github.com/astral-sh/ruff/pull/22064
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Remove some nondeterminism in constraint set tests

---

_Pull request opened by @dcreager on 2025-12-18 22:10_

We're seeing a lot of nondeterminism in the ecosystem tests at the moment, which started (or at least got worse) once `Callable` inference landed.

This PR attempts to remove this nondeterminism. We recently (https://github.com/astral-sh/ruff/pull/21983) added a `source_order` field to BDD nodes, which tracks when their constraint was added to the BDD. Since we build up constraints based on the order that they appear in the underlying source, that gives us a stable ordering even though we use an arbitrary salsa-derived ordering for the BDD variables.

The issue (at least for some of the flakiness) is that we add "derived" constraints when walking a BDD tree, and those derived constraints inherit or borrow the `source_order` of the "real" constraint that implied them. That means we can get multiple constraints in our specialization that all have the same `source_order`. If we're not careful, those "tied" constraints can be ordered arbitrarily.

The fix requires ~three~ ~four~ several steps:

- When starting to construct a sequent map (the data structure that stores the derived constraints), we first sort all of the "real" constraints by their `source_order`. That ensures that we insert things into the sequent map in a stable order.
- During sequent map construction, derived facts are discovered by a deterministic process applied to constraints in a (now) stable order. So derived facts are now also inserted in a stable order.
- We update the fields of `SequentMap` to use `FxOrderSet` instead of `FxHashSet`, so that we retain that stable insertion order.
- When walking BDD paths when constructing a specialization, we were already sorting the constraints by their `source_order`. However, we were not considering that we might get derived constraints, and therefore constraints with "ties". Because of that, we need to make sure to use a _stable_ sort, that retains the insertion order for those ties.

All together, this...should...fix the nondeterminism. (Unfortunately, I haven't been able to effectively test this, since I haven't been able to coerce local tests to flop into the other order that we sometimes see in CI.)

---

_Review requested from @carljm by @dcreager on 2025-12-18 22:10_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-18 22:10_

---

_Review requested from @sharkdp by @dcreager on 2025-12-18 22:10_

---

_Label `internal` added by @dcreager on 2025-12-18 22:11_

---

_Label `ty` added by @dcreager on 2025-12-18 22:11_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2787 on 2025-12-18 22:12_

There are other `FxHashMap`s in here, but they're only used for existence checks; we never iterate over them. The ones changed below are the only ones we iterate over, and where insertion order is therefore important.

---

_@dcreager reviewed on 2025-12-18 22:12_

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 22:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-18 22:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1221:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
+ tests/test_groupby.py:433:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
- tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(str & Any) | (bytes & Any) | (int & Any) | ... omitted 12 union elements]`
+ tests/test_resampler.py:394:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[(Any & str) | (Any & bytes) | (Any & int) | ... omitted 12 union elements]`
- Found 5079 diagnostics
+ Found 5080 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @carljm on 2025-12-18 22:57_

It's hard to tell if this fixes the issue, because if it doesn't, or if it does but it "solidifies" an ordering that doesn't match what we happen to get on the "pre" run, the results will look the same 😆 

---

_@carljm approved on 2025-12-18 22:58_

Seems reasonable, if potentially expensive?

Guess there's no way to see if it worked other than land it?

---

_Comment by @AlexWaygood on 2025-12-18 23:00_

And (regarding https://github.com/astral-sh/ruff/pull/22064#issuecomment-3672610381) we we already non-deterministic before the `Callable` PR landed, so the aim isn't even "fully deterministic again" here, it's "more deterministic than we are right now"

---

_Comment by @dcreager on 2025-12-18 23:56_

> Seems reasonable, if potentially expensive?

I hope (and codspeed CI seems to agree) that it won't be too bad — the biggest change is sorting before starting sequent map construction, and my intuition is that we're doing enough work finding all of the sequents that the extra sorting step won't be a bottleneck.

---

_Comment by @dcreager on 2025-12-18 23:57_

> more deterministic than we are right now

To be fair, where we are now is really quite non-deterministic :sweat_smile: 

---

_Merged by @dcreager on 2025-12-19 00:00_

---

_Closed by @dcreager on 2025-12-19 00:00_

---

_Branch deleted on 2025-12-19 00:00_

---
