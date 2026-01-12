```yaml
number: 22037
title: "[ty] Refactor completion generation"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-refactor
created_at: 2025-12-17T19:43:14Z
updated_at: 2025-12-18T16:00:11Z
url: https://github.com/astral-sh/ruff/pull/22037
synced_at: 2026-01-12T15:57:39Z
```

# [ty] Refactor completion generation

---

_@BurntSushi_

This refactor is intended to give more structure to how we generate
completions. There's now a `Context` for "how do we figure out what kind
of completions to offer" and also a `CollectionContext` for "how do we
figure out which completions are appropriate or not." We double down on
`Completions` as a collector and a single point of truth for this. It
now handles adding information to `Completion` (based on the context)
and also skipping completions that are inappropriate (instead of
filtering them after-the-fact).

We also bundle a bunch of state into a new `ContextCursor` type, and
then define a bunch of predicates/accessors on that type that were
previously free functions with loads of parameters.

Finally, we introduce more structure to ranking. Instead of an anonymous
tuple, we define an explicit type with some helper types to hopefully
make the influence on ranking from each constituent piece a bit clearer.

This does seem to fix one bug around detecting the target for non-import
completions, but otherwise should not have any changes in behavior.

This is meant to be a precursor to improving completion ranking.


---

_Review requested from @carljm by @BurntSushi on 2025-12-17 19:43_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-17 19:43_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-17 19:43_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-17 19:43_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-17 19:43_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-17 19:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2795 diagnostics
+ Found 2792 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 7 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`


```

</details>


No memory usage changes detected ✅



---

_Label `internal` added by @BurntSushi on 2025-12-17 19:50_

---

_Label `server` added by @BurntSushi on 2025-12-17 19:50_

---

_Label `ty` added by @BurntSushi on 2025-12-17 19:50_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-17 19:50_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-17 19:50_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-17 19:50_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-12-17 19:50_

---

_Comment by @BurntSushi on 2025-12-17 19:52_

r? @RasmusNygren (Absolutely no obligation, but you've been doing contributions to this file so figured I'd ping you on this one. Also, this will likely have massive conflicts with #22002.)

---

_Comment by @RasmusNygren on 2025-12-17 21:40_

> r? @RasmusNygren (Absolutely no obligation, but you've been doing contributions to this file so figured I'd ping you on this one. Also, this will likely have massive conflicts with #22002.)

Thanks for looping me in! I won't have time to take a thorough look until tomorrow evening at the earliest (CET) so happily land this without a review from me.

Had a quick peek though and it looks like a great improvement, especially the ground work this lays for improving sorting/ranking which seemed non-trivial to extend correctly today (pre-refactor).



---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:205 on 2025-12-18 09:54_

Would it make sense if we store `db` on `context` or is this a massive pain? 

I'm also wondering if we should use `SemanticModel` everywhere instead of using it in some code paths but use `db` + `file` in others.

---

_@MichaReiser approved on 2025-12-18 09:56_

Nice, makes sense

---

_@BurntSushi reviewed on 2025-12-18 15:30_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:205 on 2025-12-18 15:30_

I don't think it's a big pain. It's another lifetime parameter, but not a big deal.

I tried it out (and also using `SemanticModel` in more places) and wasn't quite happy with it. I think I'll leave it alone for now, but keep it in mind for the future.

---

_Merged by @BurntSushi on 2025-12-18 16:00_

---

_Closed by @BurntSushi on 2025-12-18 16:00_

---

_Branch deleted on 2025-12-18 16:00_

---
