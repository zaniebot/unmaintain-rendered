```yaml
number: 22371
title: "[ty] Allow enum narrowing for classes that don't override `__eq__`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/tag-eq
created_at: 2026-01-04T18:45:45Z
updated_at: 2026-01-06T13:30:40Z
url: https://github.com/astral-sh/ruff/pull/22371
synced_at: 2026-01-12T15:57:48Z
```

# [ty] Allow enum narrowing for classes that don't override `__eq__`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2192.


---

_Label `ty` added by @charliermarsh on 2026-01-04 18:45_

---

_Comment by @astral-sh-bot[bot] on 2026-01-04 18:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-04 18:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/rpc/telegram.py:563:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:563:62: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:569:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:569:58: error[invalid-key] Unknown key "lock_end_time" for TypedDict `RPCExitCancelMsg`: Unknown key "lock_end_time"
- freqtrade/rpc/telegram.py:573:41: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:573:41: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:576:59: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:576:59: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:579:59: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:579:59: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:582:30: error[invalid-key] Unknown key "status" for TypedDict `RPCCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:582:30: error[invalid-key] Unknown key "status" for TypedDict `RPCExitCancelMsg`: Unknown key "status"
- freqtrade/rpc/telegram.py:584:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCCancelMsg`: Unknown key "msg"
- freqtrade/rpc/telegram.py:584:30: error[invalid-key] Unknown key "msg" for TypedDict `RPCExitCancelMsg`: Unknown key "msg"
- Found 681 diagnostics
+ Found 667 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5533 diagnostics
+ Found 5528 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-05 20:59_

---

_Review requested from @carljm by @charliermarsh on 2026-01-05 20:59_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-05 20:59_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-05 20:59_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-05 20:59_

---

_Comment by @AlexWaygood on 2026-01-06 13:28_

This leads to incorrect narrowing for tagged unions that use `IntEnum`s, `StrEnum`s, or custom enums that use `int` or `str` as mixins:

```py
from typing import Literal
from enum import IntEnum, StrEnum, Enum

class Foo(IntEnum):
    X = 1

class Bar(StrEnum):
    X = "foo"

class Baz(int, Enum):
    X = 1

class Spam(str, Enum):
    X = "foo"

def f(x: tuple[int, Literal[Foo.X]] | tuple[str, Literal[1]]):
    if x[1] == 1:
        reveal_type(x)  # tuple[str, Literal[1]]

def g(x: tuple[int, Literal[Bar.X]] | tuple[str, Literal["foo"]]):
    if x[1] == "foo":
        reveal_type(x)  # tuple[str, Literal["foo"]]

def h(x: tuple[int, Literal[Baz.X]] | tuple[str, Literal[1]]):
    if x[1] == 1:
        reveal_type(x)  # tuple[str, Literal[1]]

def i(x: tuple[int, Literal[Spam.X]] | tuple[str, Literal["foo"]]):
    if x[1] == "foo":
        reveal_type(x)  # tuple[str, Literal["foo"]]
```

At runtime, `Foo.X == Baz.X == 1`, and `Bar.X == Spam.X == "foo"`, so all these narrowings are incorrect:

```pycon
>>> from enum import IntEnum, StrEnum, Enum
... 
... class Foo(IntEnum):
...     X = 1
... 
... class Bar(StrEnum):
...     X = "foo"
... 
... class Baz(int, Enum):
...     X = 1
... 
... class Spam(str, Enum):
...     X = "foo"
...         
>>> Foo.X == 1
True
>>> Bar.X == "foo"
True
>>> Baz.X == 1
True
>>> Spam.X == "foo"
True
```

The cause of the bug is that the `Type::overrides_equality` method includes `MemberLookupPolicy::MRO_NO_INT_OR_STR_LOOKUP` in its policy when trying to determine whether the class overrides `__eq__`: it returns `false` if it determines that the class inherits its `__eq__` method from `str` or `int`. This is a pre-existing issue on `main`, and the cause of this bug: https://github.com/astral-sh/ty/issues/1454. The comment in `Type::overrides_equality` states that it's okay to ignore `__eq__` methods inherited from `int` or `str` because we "know that they are well-behaved", but that's not sufficient for several callers of the method.

https://github.com/astral-sh/ruff/blob/ce059c4857c1d6eeb453cf9687fc82f8784798df/crates/ty_python_semantic/src/types.rs#L988-L1010

Ideally I feel like we'd fix that bug first before making the impact of that bug more severe... but also, maybe it doesn't come up that much, so maybe it's fine to go ahead with this even while it remains unfixed? IDK though, curious for @carljm's thoughts!

---
