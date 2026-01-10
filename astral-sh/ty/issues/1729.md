```yaml
number: 1729
title: Break salsa cycle when inferring function signatures
type: issue
state: closed
author: dcreager
labels:
  - callables
assignees: []
created_at: 2025-12-02T19:20:28Z
updated_at: 2025-12-11T20:00:20Z
url: https://github.com/astral-sh/ty/issues/1729
synced_at: 2026-01-10T01:56:40Z
```

# Break salsa cycle when inferring function signatures

---

_Issue opened by @dcreager on 2025-12-02 19:20_

While digging into an unrelated issue, I noticed that we are currently very likely to enter a salsa cycle when inferring the signature of a function literal.

The particular example I found involves the `functools.cache` decorator, whose definition is:

```py
def cache(fn: Callable[..., _T]) -> _lru_cache_wrapper[_T]:
    ...
```

Then when we infer the type of a function definition that is decorated with `@cache`:

```py
@cache
def f(x: int) -> int:
    ...
```

- We enter `infer_definition_types` for `f`.
- We construct a `Type::FunctionType` for the (undecorated) definition.
- We apply the `@cache` decorator:
  - This requires checking whether `f` is assignable to `Callable[..., T]`. To do that, we need the signature of `f`.
  - `OverloadLiteral::raw_signature` [calls](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/function.rs#L502)
  - `Signature::from_function`, which [calls](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/signatures.rs#L489)
  - `Parameters::from_parameters`, which [calls](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/signatures.rs#L1462)
  - `infer_method_information`, which [calls](https://github.com/astral-sh/ruff/blob/508c0a086181a265ce3f9012b50b6fe513b91776/crates/ty_python_semantic/src/types/signatures.rs#L72)
  - `infer_definition_types` for `f`, to check if `f` is a generic method, `@classmethod`, or `@staticmethod`.

We should see if there's a way to break that cycle. We might even consider not constructing the signatures lazily!

---

_Added to milestone `Stable` by @carljm on 2025-12-02 19:33_

---

_Comment by @carljm on 2025-12-02 20:20_

I think there's a very similar (but possibly independent?) issue that can occur with overloaded function definitions in unreachable code -- in that case the cycle goes through `is_input_function_like`. A cycle shows up in this test in `overloads.md` (run under Python 3.9), which doesn't contain any apparent cyclic references:

```py
import sys
from typing import overload

if sys.version_info < (3, 10):
    def func(x: int) -> int:
        return x

elif sys.version_info <= (3, 12):
    @overload
    def func() -> None: ...
    @overload
    def func(x: int) -> int: ...
    def func(x: int | None = None) -> int | None:
        return x

reveal_type(func)  # revealed: def func(x: int) -> int
func()  # error: [missing-argument]
```

The cycle is in the second (overloaded) definition of `func` in the unreachable code.


---

_Label `callables` added by @dhruvmanila on 2025-12-03 04:53_

---

_Comment by @mtshiba on 2025-12-04 03:41_

Alternatively, we might need to improve our cycle-normalization handling so that it doesn't matter if `signature (infer_definition_types)` enters a query cycle. See https://github.com/astral-sh/ty/issues/1732#issuecomment-3609937420.

---

_Comment by @carljm on 2025-12-04 04:24_

Ideally we would do both -- frequently-occurring cycles in places where the semantics are not inherently cyclic are expensive for performance.

---

_Comment by @dhruvmanila on 2025-12-04 12:51_

> * `infer_method_information`, which [calls](https://github.com/astral-sh/ruff/blob/508c0a086181a265ce3f9012b50b6fe513b91776/crates/ty_python_semantic/src/types/signatures.rs#L72)

Can you clarify how will it reach here? Because, the `infer_method_information` should short-circuit because `f` is not a method [here](https://github.com/astral-sh/ruff/blob/508c0a086181a265ce3f9012b50b6fe513b91776/crates/ty_python_semantic/src/types/signatures.rs#L64) i.e., it should never reach the `infer_definition_types` for `f`.

For context, I'm trying to see why am I seeing `Divergent` type in the following code on https://github.com/astral-sh/ruff/pull/21445:
```py
from typing import Callable

def decorator[**P](c: Callable[P, int]) -> Callable[P, str]: ...

@decorator
def func(a: int) -> int: ...

# ((a: int) -> str) | ((a: Divergent) -> str)
reveal_type(func)
```

And, I thought I might be facing a similar issue as this so I'm trying to see where the cycle is present.


---

_Comment by @dhruvmanila on 2025-12-04 13:30_

Ok, I think I know where the cycle is:

- We enter `infer_definition_types` for `f`.
- We construct a `Type::FunctionType` for the (undecorated) definition.
- We apply the `@cache` decorator:
  - This requires checking whether `f` is assignable to `Callable[..., T]`. To do that, we need the signature of `f`.
  - `OverloadLiteral::raw_signature` [calls](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/function.rs#L502)
  - `Signature::from_function`, which [calls](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/signatures.rs#L489)
  - `Parameters::from_parameters`, which [calls](https://github.com/astral-sh/ruff/blob/3aefe85b32ff698b1a2086c2b50ff38af5c9dbed/crates/ty_python_semantic/src/types/signatures.rs#L1515)
  - `Parameter::from_node_and_kind` for `int` annotated type, which [calls](https://github.com/astral-sh/ruff/blob/3aefe85b32ff698b1a2086c2b50ff38af5c9dbed/crates/ty_python_semantic/src/types/signatures.rs#L1985)
  - `definition_expression_type`, which [calls](https://github.com/astral-sh/ruff/blob/3aefe85b32ff698b1a2086c2b50ff38af5c9dbed/crates/ty_python_semantic/src/types.rs#L190)
  - `infer_definition_types` for `f` because the same `Definition` is being passed around

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-05 10:59_

---

_Added to milestone `Beta` by @MichaReiser on 2025-12-05 10:59_

---

_Comment by @dcreager on 2025-12-05 14:56_

I have an initial stab at this going in https://github.com/astral-sh/ruff/pull/21813

---

_Assigned to @dcreager by @dhruvmanila on 2025-12-05 15:07_

---

_Unassigned @dcreager by @MichaReiser on 2025-12-10 16:20_

---

_Assigned to @dhruvmanila by @MichaReiser on 2025-12-10 16:20_

---

_Comment by @MichaReiser on 2025-12-10 16:21_

From what I understood, @dhruvmanila's now working on this

---

_Closed by @dcreager on 2025-12-11 20:00_

---
