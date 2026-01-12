```yaml
number: 21614
title: "[ty] Retain the function-like-ness of `Callable` types when binding `self`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/callable-bind-self
created_at: 2025-11-24T14:07:24Z
updated_at: 2025-11-24T21:14:05Z
url: https://github.com/astral-sh/ruff/pull/21614
synced_at: 2026-01-12T15:57:29Z
```

# [ty] Retain the function-like-ness of `Callable` types when binding `self`

---

_@AlexWaygood_

## Summary

For something like this:

```py
from typing import Callable

def my_lossy_decorator(fn: Callable[..., int]) -> Callable[..., int]:
    return fn

class MyClass:
    @my_lossy_decorator
    def method(self) -> int:
        return 42
```

we will currently infer the type of `MyClass.method` as a function-like `Callable`, but we will infer the type of `MyClass().method` as a `Callable` that is _not_ function-like. That's because a `CallableType` currently "forgets" whether it was function-like or not during the `bound_self` transformation:

https://github.com/astral-sh/ruff/blob/a57e29131125bf05db7379e90c7616eec32624fe/crates/ty_python_semantic/src/types.rs#L10985-L10987

This seems incorrect, and it's quite different to what we do when binding the `self` parameter of `FunctionLiteral` types: `BoundMethod` types are all seen as subtypes of function-like `Callable` supertypes -- here's `BoundMethodType::into_callable_type`:

https://github.com/astral-sh/ruff/blob/a57e29131125bf05db7379e90c7616eec32624fe/crates/ty_python_semantic/src/types.rs#L10844-L10860

The bug here is also causing lots of false positives in the ecosystem report on https://github.com/astral-sh/ruff/pull/21611: a decorated method on a subclass is currently not seen as validly overriding an undecorated method with the same signature on a superclass, because the undecorated superclass method is seen as function-like after binding `self` whereas the decorated subclass method is not.

Fixing the bug required adding a new API in `protocol_class.rs`, because it turns out that for our purposes in protocol subtyping/assignability, we really do want a callable type to forget its function-like-ness when binding `self`.

I initially tried out this change without changing anything in `protocol_class.rs`. However, it resulted in many ecosystem false positives and new false positives on the typing conformance test suite. This is because it would mean that no protocol with a `__call__` method would ever be seen as a subtype of a `Callable` type, since the `__call__` method on the protocol would be seen as being function-like whereas the `Callable` type would not be seen as function-like.

## Test Plan

Added an mdtest that fails on `main`


---

_Label `ty` added by @AlexWaygood on 2025-11-24 14:07_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 14:07_

---

_Label `ty` added by @AlexWaygood on 2025-11-24 14:07_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 14:07_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 14:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 14:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_raw/wrapper.py:102:13: error[unresolved-attribute] Object of type `(...) -> object` has no attribute `__get__`
- src/antidote/core/_raw/wrapper.py:136:13: error[unresolved-attribute] Object of type `(...) -> Awaitable[object]` has no attribute `__get__`
+ src/antidote/core/_raw/wrapper.py:102:13: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `((...) -> object) | ((...) -> object)`
+ src/antidote/core/_raw/wrapper.py:136:13: warning[possibly-missing-attribute] Attribute `__get__` may be missing on object of type `((...) -> Awaitable[object]) | ((...) -> Awaitable[object])`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/debugging/probe/test_remoteconfig.py:69:9: error[invalid-assignment] Object of type `(...) -> Iterable[Probe]` is not assignable to attribute `get_probes` of type `(...) -> Iterable[Probe]`
- Found 8243 diagnostics
+ Found 8242 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/runtime.py:745:9: error[invalid-assignment] Object of type `(() -> Declarations) | Unknown` is not assignable to attribute `__egg_decls_thunk__` of type `() -> Declarations`
- Found 1493 diagnostics
+ Found 1492 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 14:16_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 0 | 2 | 0 |
| `possibly-missing-attribute` | 2 | 0 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| **Total** | **2** | **4** | **0** |

**[Full report with detailed diff](https://alex-callable-bind-self.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-callable-bind-self.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-24 14:23_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-24 14:23_

---

_Comment by @AlexWaygood on 2025-11-24 15:56_

The ecosystem diff shows two false-positive diagnostics going away. It also shows two pre-existing diagnostics becoming slightly more confusing due to the fact that we don't have a great way of distinguishing between function-like callables and non-function-like callables in our `Type` display yet.

---

_Marked ready for review by @AlexWaygood on 2025-11-24 15:56_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-24 15:56_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-24 15:56_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-24 15:56_

---

_Converted to draft by @AlexWaygood on 2025-11-24 20:10_

---

_Marked ready for review by @AlexWaygood on 2025-11-24 20:13_

---

_@sharkdp approved on 2025-11-24 21:08_

Thank you!

---

_Merged by @AlexWaygood on 2025-11-24 21:14_

---

_Closed by @AlexWaygood on 2025-11-24 21:14_

---

_Branch deleted on 2025-11-24 21:14_

---
