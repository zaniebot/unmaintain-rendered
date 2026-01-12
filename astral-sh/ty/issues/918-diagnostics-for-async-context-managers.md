```yaml
number: 918
title: Diagnostics for async context managers
type: issue
state: closed
author: sharkdp
labels:
  - good first issue
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-07-31T10:49:54Z
updated_at: 2025-08-05T14:41:38Z
url: https://github.com/astral-sh/ty/issues/918
synced_at: 2026-01-12T15:54:24Z
```

# Diagnostics for async context managers

---

_@sharkdp_

For *synchronous* context managers, we emit diagnostics when the context manager is not implemented properly. For example, if there is no `__enter__` method, or no `__exit__` method. Or if the call to these methods fails.

https://play.ty.dev/a64092ee-fde8-40a6-9e32-56c84602e677
```py
class Ctx:
    def __enter__(self) -> str:  # remove this to generate a diagnostic
        return "a"

    def __exit__(self, exc_type, exc_val, exc_tb):  # change the signature here to generate a diagnostic
        pass


with Ctx() as c:
    pass
```

We should do the same for async context managers.

```py
class Ctx:
    async def __aenter__(self) -> str:  # removing this does not yet generate a diagnostic
        return "a"

    async def __aexit__(self, exc_type, exc_val, exc_tb):  # changing the signature here does not yet generate a diagnostic
        pass


async def main():
    async with Ctx() as c:
        pass
```

* The entry-point to implement this is https://github.com/astral-sh/ruff/blob/27b03a9d7bf779049585bb6cfbf229f18352572d/crates/ty_python_semantic/src/types.rs#L4859C1-L4859C83
* We could unify this method with `try_enter` and add a `EvaluationMode` parameter, similar to what we do for [`try_iterate`](https://github.com/astral-sh/ruff/blob/27b03a9d7bf779049585bb6cfbf229f18352572d/crates/ty_python_semantic/src/types.rs#L4640-L4648)
* This would allow us to re-use `ContextManagerError`. We would probably need to add a `mode` parameter to that error type in order to adapt the error messages to mention `__aenter__` instead of `__enter__` etc. Similar adaptations have been made for `IterationError` in https://github.com/astral-sh/ruff/pull/19634
* See existing tests for synchronous context managers: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/with/sync.md
* The new tests should be added here: https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/with/async.md
* 

---

_Label `good first issue` added by @sharkdp on 2025-07-31 10:49_

---

_Label `diagnostics` added by @sharkdp on 2025-07-31 10:49_

---

_Label `help wanted` added by @carljm on 2025-08-05 02:22_

---

_Closed by @carljm on 2025-08-05 14:41_

---
