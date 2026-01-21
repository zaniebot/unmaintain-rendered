```yaml
number: 22718
title: "[ty] Support recursive and stringified annotations in functional `typing.NamedTuple`s"
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: alex/defer-namedtuple
created_at: 2026-01-19T11:42:59Z
updated_at: 2026-01-21T15:01:06Z
url: https://github.com/astral-sh/ruff/pull/22718
synced_at: 2026-01-21T16:04:59Z
```

# [ty] Support recursive and stringified annotations in functional `typing.NamedTuple`s

---

_@AlexWaygood_

Fixes https://github.com/astral-sh/ty/issues/2528, fixes https://github.com/astral-sh/ty/issues/2529.

In order to support `typing.NamedTuple`s with recursive or stringified types in their field specs, we must defer inference of the second argument to `typing.NamedTuple()` calls. However, we don't need to defer inference of `collections.namedtuple()` calls (those calls don't expect type expressions -- they just expect a string literal or a sequence of string literals), and nor do we need to defer inference of "dangling" `NamedTuple` calls. Dangling `NamedTuple` classes can be recursive, but only in the context of when they appear inside a class's bases list, and we already defer inference of all expressions inside a class's bases list.

## Test plan

Mdtests updated and extended

Co-authored-by @charliermarsh (picking up from #22627)

---

_Label `ty` added by @AlexWaygood on 2026-01-19 11:43_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-19 11:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:45_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | T@resolve_block_document_references | dict[str, Any]]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, Unknown | T@resolve_variables]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[Unknown | T@resolve_variables]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5407 diagnostics
+ Found 5412 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/tests/utils/mock.py:74:39: error[invalid-argument-type] Invalid `NamedTuple()` field definition: Expected a `(name, type)` tuple, found `Literal["version"]`
+ rotkehlchen/tests/utils/mock.py:74:38: error[invalid-named-tuple] Invalid argument to parameter `fields` of `NamedTuple()`: `fields` must be a sequence of literal lists or tuples

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14462 diagnostics
+ Found 14463 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-19 11:49_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 4 | 1 | 2 |
| `invalid-argument-type` | 0 | 1 | 1 |
| `invalid-named-tuple` | 1 | 0 | 0 |
| **Total** | **5** | **2** | **3** |


**[Full report with detailed diff](https://81a5f361.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://81a5f361.ty-ecosystem-ext.pages.dev/timing))



---

_@AlexWaygood reviewed on 2026-01-19 12:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:179 on 2026-01-19 12:41_

This must be resolved before the PR will be mergeable... (this is just as much a problem on https://github.com/astral-sh/ruff/pull/22627, but only after you've merged `main` into that PR branch, interestingly)

---

_Comment by @AlexWaygood on 2026-01-19 14:39_

There's still a stack overflow on this PR (https://github.com/astral-sh/ruff/pull/22718#discussion_r2704622488), and I'll see if I can come up with a fix for that, but I'll open this up for review in case it's very obvious to anybody else how to fix it...!

---

_Marked ready for review by @AlexWaygood on 2026-01-19 14:39_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-19 14:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-19 14:39_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-19 14:39_

---

_Review requested from @charliermarsh by @AlexWaygood on 2026-01-19 14:39_

---

_Comment by @AlexWaygood on 2026-01-19 14:48_

Okay, I think I fixed the stack overflow.

---

_@charliermarsh approved on 2026-01-19 15:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:1 on 2026-01-19 15:08_

Unfortunately a lot of the diff on this file is still just code moving around. I tried to keep a fairly atomic commit history on this PR, though; it may be easiest to review commit-by-commit.

---

_@AlexWaygood reviewed on 2026-01-19 15:08_

---

_Comment by @charliermarsh on 2026-01-19 15:11_

Looks great.

Are we certain there are no other cases in which a dangling call could make a recursive reference? What about, e.g.:

```python
from typing import NamedTuple

X = type("X", (NamedTuple("NT", [("field", "X | int")]),), {})
```


---

_Comment by @AlexWaygood on 2026-01-19 15:15_

> Looks great.
> 
> Are we certain there are no other cases in which a dangling call could make a recursive reference? What about, e.g.:
> 
> ```python
> from typing import NamedTuple
> 
> X = type("X", (NamedTuple("NT", [("field", "X | int")]),), {})
> ```

Nice. That causes a panic on this branch 🙃

I think what that means is that we may have to defer inference of the bases tuple passed to `type()` calls as well...

---

_Comment by @AlexWaygood on 2026-01-19 15:18_

> I think what that means is that we may have to defer inference of the bases tuple passed to `type()` calls as well...

I'm sort-of inclined to do that as a followup? It's a real bug that we should fix, but it also feels very unlikely to come up in real code, and it'll make the diff on this PR much bigger.

I would add an x-failing mdtest with the `<!-- expect-panic -->` HTML comment, but it looks like the mdtest would be really slow, so I don't think it's worth it.

---

_Comment by @AlexWaygood on 2026-01-19 15:27_

And the panic isn't new -- it's a pre-existing issue. I can trigger it on `main` with:

```py
X = type("X", (tuple["X | None"],), {})
```

So we do just need to defer inference of the bases tuple passed to `type()`. I opened https://github.com/astral-sh/ty/issues/2564.

---

_Comment by @charliermarsh on 2026-01-19 15:33_

Sounds good 👍 

---

_Comment by @charliermarsh on 2026-01-19 15:33_

I suspect I can fix that panic once this merges given the patterns established here (unless you're on it).

---

_Comment by @AlexWaygood on 2026-01-19 15:34_

> I suspect I can fix that panic once this merges given the patterns established here (unless you're on it).

yeah, that would be great!

---

_Comment by @AlexWaygood on 2026-01-19 15:35_

(this PR _is_ one that I'd love @carljm and/or @dcreager to take a quick look at 😅)

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:182 on 2026-01-21 14:14_

super nit

```suggestion
When `NamedTuple` is used directly as a base class without being assigned to a variable first, it's a
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:231 on 2026-01-21 14:19_

This means the `X` in `next`'s type annotation, not the `X` that names the `NamedTuple`, right?


```suggestion
# The string "X" in "next"'s type refers to the internal name, not "BadNode", so it won't resolve:
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:5717 on 2026-01-21 14:23_

I like how the cycle function is right next to the tracked function itself. 

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:5785 on 2026-01-21 14:25_

Does this suggest that it's okay to have a dangling call outside of a base class list, but then the call _cannot_ have forward/string references?

Something like

```py
def foo(base):
    class Point(base): ...
    return Point

foo(NamedTuple("Good", (("x", int), ("y", int))))             # okay
foo(NamedTuple("Bad", (("x", "Forward"), ("y", "Forward"))))  # bad

class Forward: ...
```

(Or are dangling calls only allowed in base class lists?)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:1743 on 2026-01-21 14:49_

I was going to suggest making `seen` a field of the SpecBuilder, to reduce the churn on these method signatures. But on second thought I'm not sure it would be worth it — the naive approach would mean that if we call `infer_map` more than once with the same formal/actual type, we would skip all inference work for the second and later calls. But the result is important, since we use it to produce "invalid argument" diagnostics. So you'd have to make `seen` a map, and store the method result, so that we still return the same result for later (cached) calls. And at that point I'm not convinced it's worth the effort just to avoid a new method argument.

---

_@dcreager approved on 2026-01-21 14:50_

No concerns with the implementation, just some clarifying questions for my own curiosity

---

_@AlexWaygood reviewed on 2026-01-21 14:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:5785 on 2026-01-21 14:54_

Dangling calls are allowed in arbitrary locations, but I _think_ they can only cause _problematic cycles_ if they appear in class bases or other locations where we know we will need to defer inference (such as the bases list passed to `type()` calls -- see https://github.com/astral-sh/ty/issues/2564).

I would not describe myself as 100% confident that this is correct, so if you can think of any other examples where this PR branch stack-overflows or panics, I'd love to know about them 😆

---
