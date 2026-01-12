```yaml
number: 19099
title: "[ty] remove any_over_type"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
base: main
head: cjm/noanyover
created_at: 2025-07-02T18:43:35Z
updated_at: 2025-07-03T16:43:11Z
url: https://github.com/astral-sh/ruff/pull/19099
synced_at: 2026-01-12T15:56:31Z
```

# [ty] remove any_over_type

---

_@carljm_

## Summary

Remove the recursive type walk `any_over_type` and its sole current use in deciding whether to emit a redundant-cast diagnostic; instead, just skip the redundant-cast diagnostic if we are casting `Unknown` or `Todo` as a top-level type. This removes another recursive type walk (and its associated recursive-type complications).

The ecosystem report suggests the number of false positives this currently adds is quite manageable, and the few that it does add can mostly be removed by targeting a few cases where we currently create `Todo` types. While suppressing false positives from `Todo` types is useful in the short term and worth doing when it's easy, I don't think it should drive significant architecture decisions or extra runtime work.

If we do decide (even though it currently seems to have little impact on the ecosystem report) that it's important for gradual-guarantee reasons to do deep suppression of `redundant-cast` when nested `Unknown` types are involved (I am not sure how strong the case for this is, since the use of `cast` in the first place suggests a typed codebase), I think the better approach to this would be a strategy parameter to `is_equivalent_to` that could cause `Unknown` to not be considered equivalent to `Unknown` or `Any`, rather than a separate recursive type walk.

## Test Plan

CI


---

_Label `ty` added by @carljm on 2025-07-02 18:43_

---

_Comment by @github-actions[bot] on 2025-07-02 18:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~49MB
+     memo fields = ~54MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~54MB
+     memo fields = ~60MB

werkzeug (https://github.com/pallets/werkzeug)
-     memo fields = ~129MB
+     memo fields = ~142MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ warning[redundant-cast] src/hydra_zen/_utils/coerce.py:107:16: Value is already of type `_T`
+ warning[redundant-cast] src/hydra_zen/_utils/coerce.py:178:16: Value is already of type `_T`
- Found 599 diagnostics
+ Found 601 diagnostics
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

discord.py (https://github.com/Rapptz/discord.py)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~251MB

kopf (https://github.com/nolar/kopf)
+ warning[redundant-cast] kopf/_cogs/configs/diffbase.py:52:19: Value is already of type `dict[Any, Any]`
- Found 130 diagnostics
+ Found 131 diagnostics

mkosi (https://github.com/systemd/mkosi)
-     memo fields = ~106MB
+     memo fields = ~97MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~66MB
+     memo fields = ~72MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~156MB

mkdocs (https://github.com/mkdocs/mkdocs)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB

streamlit (https://github.com/streamlit/streamlit)
+ warning[redundant-cast] lib/streamlit/runtime/state/common.py:71:5: Value is already of type `tuple[@Todo(Inference of subscript on special form), ...]`
- Found 3304 diagnostics
+ Found 3305 diagnostics

meson (https://github.com/mesonbuild/meson)
+ warning[redundant-cast] mesonbuild/options.py:705:35: Value is already of type `list[@Todo(Inference of subscript on special form)]`
+ warning[redundant-cast] mesonbuild/options.py:772:35: Value is already of type `list[@Todo(Inference of subscript on special form)]`
- Found 930 diagnostics
+ Found 932 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ warning[redundant-cast] src/prefect/server/orchestration/core_policy.py:68:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
+ warning[redundant-cast] src/prefect/server/orchestration/core_policy.py:106:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
+ warning[redundant-cast] src/prefect/server/orchestration/core_policy.py:146:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
+ warning[redundant-cast] src/prefect/server/orchestration/core_policy.py:184:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
+ warning[redundant-cast] src/prefect/server/orchestration/core_policy.py:218:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
+ warning[redundant-cast] src/prefect/server/orchestration/global_policy.py:69:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
+ warning[redundant-cast] src/prefect/server/orchestration/global_policy.py:102:16: Value is already of type `list[@Todo(unsupported nested subscript in type[X])]`
- Found 3851 diagnostics
+ Found 3858 diagnostics

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-07-02 18:53_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnoanyover?runnerMode=WallTime)

### Merging #19099 will **not alter performance**

<sub>Comparing <code>cjm/noanyover</code> (ca0e578) with <code>main</code> (0660188)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Renamed from "[experiment] remove any_over_type" to "[ty] remove any_over_type" by @carljm on 2025-07-02 19:06_

---

_Marked ready for review by @carljm on 2025-07-02 19:07_

---

_Review requested from @AlexWaygood by @carljm on 2025-07-02 19:07_

---

_Review requested from @sharkdp by @carljm on 2025-07-02 19:07_

---

_Review requested from @dcreager by @carljm on 2025-07-02 19:07_

---

_@AlexWaygood reviewed on 2025-07-03 11:49_

I love the reduction in code complexity (especially relative to https://github.com/astral-sh/ruff/pull/19094!) but I do think this pretty clearly breaks the gradual guarantee for something like this?

```py
from unresolvable_module import Foo, Bar

def f(x: list[Foo]):
    y = cast(list[Bar], x)
```

it seems just like somewhat random luck that we don't have anything in the mypy_primer corpus that triggers a false-positive `redundant-cast` diagnostic on that as a result of this change :-) I'd be curious to see what this idea looks like; it seems viable:

> I think the better approach to this would be a strategy parameter to is_equivalent_to that could cause `Unknown` to not be considered equivalent to `Unknown` or `Any`, rather than a separate recursive type walk.

I also don't think this gets us away in the long term from having to find a way to recursively walk types. For example, we've discussed for a while that a feature many people want is a way to know what percentage of types across a single run of ty are inferred as `Any`/`Unknown`. Mypy's version of this feature also tells users what percentage of types are "partially `Any`/`Unknown`"; it would be a shame not to be able to match that capability.

---

_Comment by @AlexWaygood on 2025-07-03 12:40_

> I'd be curious to see what this idea looks like; it seems viable:
> 
> > I think the better approach to this would be a strategy parameter to is_equivalent_to that could cause `Unknown` to not be considered equivalent to `Unknown` or `Any`, rather than a separate recursive type walk.

But on second thought, I don't think this approach works for protocols that have unknown fields? Equivalence for protocols just normalizes both the l.h.s. and the r.h.s., then compares for identity; this has the advantage that once recursive protocols are "solved" in `Type::normalized()`, it means we don't have to worry about them in equivalence checks between protocols:

https://github.com/astral-sh/ruff/blob/fc43d3c83ec170e58c9c42ab1d98866572fe1750/crates/ty_python_semantic/src/types/instance.rs#L259-L261

But this means that these two protocols will normalize to the same type, because `Unknown`/`Todo` both normalize to `Any`, so you'd get a false-positive `redundant-cast` diagnostic if you tried to cast from one to the other. `Unknown.is_equivalent_to(Any)` would never be called:

```py
from unresolved_module import Foo
from typing import Any, Protocol

class Bar(Protocol):
    x: Foo

class Baz(Protocol):
    x: Any
```

---

_Comment by @carljm on 2025-07-03 16:43_

Ok, I'm convinced, both that we may have other needs for this in future, and that doing this `cast` check in `is_equivalent_to` won't work in all cases.

---

_Closed by @carljm on 2025-07-03 16:43_

---
