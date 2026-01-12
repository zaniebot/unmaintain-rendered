```yaml
number: 18384
title: "[ty] Fix broken property tests for disjointness"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/property-test-failure
created_at: 2025-05-30T12:28:04Z
updated_at: 2025-07-25T18:33:52Z
url: https://github.com/astral-sh/ruff/pull/18384
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Fix broken property tests for disjointness

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/549

This assertion passes on `main`:

```py
static_assert(is_disjoint_from(bool, Callable[..., Any]))
```

But this one does not:

```py
static_assert(is_disjoint_from(Callable[..., Any], bool))
```

https://play.ty.dev/d0a6b6c2-e492-4330-a866-2c4b0d51085a

The reason is that the `self.member_lookup_with_policy()` call here is incorrect. `self` might not be the `NominalInstance` type in the `match`; it might be the `Callable` type:

https://github.com/astral-sh/ruff/blob/ad2f667ee4323dd0c12224338734d722d13d28b4/crates/ty_python_semantic/src/types.rs#L2180-L2192

I also removed an invalid TODO comment, as per the conversation in https://github.com/astral-sh/ruff/pull/18368#discussion_r2114886040

## Test Plan

- Added mdtests
- `QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` passes again


---

_Review requested from @carljm by @AlexWaygood on 2025-05-30 12:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-30 12:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-30 12:28_

---

_Label `internal` added by @AlexWaygood on 2025-05-30 12:28_

---

_Label `ty` added by @AlexWaygood on 2025-05-30 12:28_

---

_Comment by @AlexWaygood on 2025-05-30 12:28_

(I added the "internal" label because the bug being fixed here hasn't appeared in any release, so this doesn't deserve an inclusion in the release notes.)

---

_Comment by @github-actions[bot] on 2025-05-30 12:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ error[unresolved-attribute] beartype/_util/hint/pep/proposal/pep612.py:596:9: Type `(...) -> Unknown` has no attribute `__name__`
- Found 575 diagnostics
+ Found 576 diagnostics

starlette (https://github.com/encode/starlette)
+ error[unresolved-attribute] starlette/config.py:139:81: Type `(Any, /) -> Any` has no attribute `__name__`
- Found 177 diagnostics
+ Found 178 diagnostics

ignite (https://github.com/pytorch/ignite)
+ error[not-iterable] ignite/handlers/base_logger.py:52:29: Object of type `list[Unknown] | ((str, Unknown, /) -> bool)` may not be iterable
- Found 2222 diagnostics
+ Found 2223 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[unsupported-operator] src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `int | (((str | None, /) -> int | None) & ~None) | (Unknown & ~None)` with `Literal[0]`
+ error[unsupported-operator] src/werkzeug/utils.py:508:12: Operator `>` is not supported for types `(str | None, /) -> int | None` and `Literal[0]`, in comparing `int | ((str | None, /) -> int | None) | (Unknown & ~None)` with `Literal[0]`
- error[invalid-assignment] src/werkzeug/utils.py:512:9: Object of type `int | (((str | None, /) -> int | None) & ~None) | (Unknown & ~None)` is not assignable to attribute `max_age` of type `int | None`
+ error[invalid-assignment] src/werkzeug/utils.py:512:9: Object of type `int | ((str | None, /) -> int | None) | (Unknown & ~None)` is not assignable to attribute `max_age` of type `int | None`
- warning[unused-ignore-comment] src/werkzeug/utils.py:513:45: Unused blanket `type: ignore` directive
- Found 452 diagnostics
+ Found 451 diagnostics

nox (https://github.com/wntrblm/nox)
- warning[possibly-unbound-attribute] nox/registry.py:107:26: Attribute `__name__` on type `(((...) -> Any) & ~None) | Func` is possibly unbound
- warning[possibly-unbound-attribute] nox/registry.py:120:23: Attribute `__name__` on type `(((...) -> Any) & ~None) | Func` is possibly unbound
+ error[unresolved-attribute] nox/registry.py:107:26: Type `((...) -> Any) | Func` has no attribute `__name__`
+ error[unresolved-attribute] nox/registry.py:120:23: Type `((...) -> Any) | Func` has no attribute `__name__`

websockets (https://github.com/aaugustin/websockets)
+ error[unresolved-attribute] src/websockets/server.py:110:17: Type `(ServerProtocol, Sequence[Unknown], /) -> Unknown | None` has no attribute `__get__`
- Found 109 diagnostics
+ Found 110 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `Unknown | Sequence[int] | int | (((int, /) -> Sequence[int] | int) & ~None)`
+ error[invalid-argument-type] test/datasets_utils.py:835:52: Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `Unknown | Sequence[int] | int | ((int, /) -> Sequence[int] | int)`

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[invalid-assignment] src/hydra_zen/third_party/beartype.py:125:9: Implicit shadowing of function `__init__`
- Found 639 diagnostics
+ Found 640 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] src/_pytest/fixtures.py:1022:42: Argument to function `_eval_scope_callable` is incorrect: Expected `(str, Config, /) -> Unknown`, found `Scope | (Unknown & ~None) | (((str, Config, /) -> Unknown) & ~None)`
+ error[invalid-argument-type] src/_pytest/fixtures.py:1022:42: Argument to function `_eval_scope_callable` is incorrect: Expected `(str, Config, /) -> Unknown`, found `Scope | (Unknown & ~None) | ((str, Config, /) -> Unknown)`
- error[invalid-argument-type] src/_pytest/fixtures.py:1385:9: Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | Sequence[object] | (((Any, /) -> object) & ~None) | tuple[Unknown, ...]`
+ error[invalid-argument-type] src/_pytest/fixtures.py:1385:9: Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | Sequence[object] | ((Any, /) -> object) | tuple[Unknown, ...]`
- error[invalid-argument-type] src/_pytest/fixtures.py:1385:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[object] | (((Any, /) -> object) & ~None)`
+ error[invalid-argument-type] src/_pytest/fixtures.py:1385:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Sequence[object] | ((Any, /) -> object)`
- error[invalid-argument-type] src/_pytest/python.py:1380:13: Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | Iterable[object] | (((Any, /) -> object) & ~None)`
+ error[invalid-argument-type] src/_pytest/python.py:1380:13: Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | Iterable[object] | ((Any, /) -> object)`

streamlit (https://github.com/streamlit/streamlit)
+ warning[possibly-unbound-attribute] lib/streamlit/runtime/fragment.py:171:47: Attribute `__qualname__` on type `Unknown | F` is possibly unbound
+ error[call-non-callable] lib/streamlit/runtime/fragment.py:245:38: Object of type `F` is not callable
+ error[call-non-callable] lib/streamlit/runtime/metrics_util.py:443:22: Object of type `F` is not callable
- Found 3271 diagnostics
+ Found 3274 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:227:38: Attribute `__name__` on type `(((Any, /) -> bool) & ~None) | ((_) -> Unknown)` is possibly unbound
+ error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:227:38: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:257:38: Attribute `__name__` on type `(((Any, /) -> bool) & ~None) | ((_) -> Unknown)` is possibly unbound
+ error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:257:38: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:313:41: Attribute `__name__` on type `(((Any, /) -> bool) & ~None) | ((_) -> Unknown)` is possibly unbound
+ error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:313:41: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:332:34: Attribute `__name__` on type `(((Any, /) -> bool) & ~None) | ((_) -> Unknown)` is possibly unbound
+ error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:332:34: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`
- warning[possibly-unbound-attribute] ddtrace/debugging/_signal/utils.py:358:37: Attribute `__name__` on type `(((Any, /) -> bool) & ~None) | ((_) -> Unknown)` is possibly unbound
+ error[unresolved-attribute] ddtrace/debugging/_signal/utils.py:358:37: Type `((Any, /) -> bool) | ((_) -> Unknown)` has no attribute `__name__`

sympy (https://github.com/sympy/sympy)
+ error[invalid-argument-type] sympy/assumptions/refine.py:63:30: Argument is incorrect: Expected `Boolean`, found `Unknown | Literal[True]`
- Found 18553 diagnostics
+ Found 18554 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-05-30 12:41_

Pretty interesting that there's a fair amount of mypy_primer fallout here, given that there was none on https://github.com/astral-sh/ruff/pull/18368! I'll go through it in a moment

---

_Comment by @AlexWaygood on 2025-05-30 12:57_

> ```diff
> + error[unresolved-attribute] beartype/_util/hint/pep/proposal/pep612.py:596:9: Type `(...) -> Unknown` has no attribute `__name__`
> ```

Here's a minimal repro of this hit. On `main` our behaviour is:

```py
from typing import Callable, reveal_type

def f(func: Callable | None):
    x = reveal_type(func).__name__ if func is not None else 'bar'  # revealed: `((...) -> Unknown) & ~None`
    reveal_type(x)  # revealed: `@Todo(map_with_boundness: intersections with negative contributions) | Literal["bar"]`
```

This is because we do not infer that `Callable` is disjoint from `None`. With this PR branch, we instead have this behaviour, which seems much more correct:

```py
from typing import Callable, reveal_type

def f(func: Callable | None):
    # error: Type `(...) -> Unknown` has no attribute `__name__`
    x = reveal_type(func).__name__ if func is not None else 'bar'  # revealed: `((...) -> Unknown)`
    reveal_type(x)  # revealed: `Unknown | Literal["bar"]`
```

There's an open question about whether we should infer all `Callable` types as having the same attributes as `types.FunctionType` instances, the same as mypy/pyright, but that should definitely not be solved in this PR.

The [`starlette`](https://github.com/encode/starlette/blob/739321d719db5b6e3fff9ccca9a2ccc7c2e07f18/starlette/config.py#L128-L139) and [`websockets`](https://github.com/python-websockets/websockets/blob/fc7cafea01ebe3ec1738160ac62889fd80653255/src/websockets/server.py#L104-L111) hits appear to be the same thing as `beartype`: we didn't understand that `Callable` types were disjoint from `None`; now we do.

---

_Comment by @AlexWaygood on 2025-05-30 13:00_

> ```diff
> + error[not-iterable] ignite/handlers/base_logger.py:52:29: Object of type `list[Unknown] | ((str, Unknown, /) -> bool)` may not be iterable
> ```

This one appears to be due to missing support for `TypeIs` -- once we support `TypeIs`, we'll be able to do `~Callable` narrowing in the `else` branch here: https://github.com/pytorch/ignite/blob/adbb260bfb1756c106fed2e793e1b645918ece27/ignite/handlers/base_logger.py#L44-L54

---

_Comment by @AlexWaygood on 2025-05-30 13:10_

The [sympy](https://github.com/sympy/sympy/blob/c5ec3a7ce41a99d7a7397676fdf6c8e050a918a8/sympy/assumptions/refine.py#L11-L63) one looks correct to me. It's yet another case where we didn't see `Callable` types and `None` as being disjoint, but now we do. The function checks that the `handler` variable is `not None`, which narrows the type of `handler` (returned from the `handlers_dict.get()` call to `Callable[[Expr, Boolean], Expr]`. But the function then uses the `assumptions` variable as the second argument when it calls the `handler` callback, and the `assumptions` variable could still be `Literal[True]`.

So I think all the mypy_primer hits are correct, and show that this PR improves our inference overall!

---

_@carljm approved on 2025-05-30 15:31_

Thank you!

---

_Merged by @AlexWaygood on 2025-05-30 15:49_

---

_Closed by @AlexWaygood on 2025-05-30 15:49_

---

_Branch deleted on 2025-05-30 15:49_

---

_Branch restored on 2025-07-25 18:27_

---

_Branch deleted on 2025-07-25 18:33_

---
