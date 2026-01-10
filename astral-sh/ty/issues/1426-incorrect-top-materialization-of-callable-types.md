---
number: 1426
title: "Incorrect top materialization of `Callable` types"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - set-theoretic types
assignees: []
created_at: 2025-10-23T20:03:06Z
updated_at: 2025-12-24T17:49:11Z
url: https://github.com/astral-sh/ty/issues/1426
synced_at: 2026-01-10T01:52:52Z
---

# Incorrect top materialization of `Callable` types

---

_Issue opened by @AlexWaygood on 2025-10-23 20:03_

### Summary

Because of [this TODO](https://github.com/astral-sh/ruff/blob/83a3bc4ee94de552d5cec9a3146aff00dade6903/crates/ty_python_semantic/src/types/signatures.rs#L1448-L1453), we currently believe that the top materialization of `Callable[..., Any]` is `Callable[[], object]`. But that's not correct, and it means that we infer bad types when inferring the narrowed type after `callable()` calls, for example (because `callable()` returns `TypeIs`, and following https://github.com/astral-sh/ruff/pull/20591 we implicitly convert `TypeIs` types to their top materializations):

```py
from typing import Any
from typing import Callable

def f(x: Callable[..., Any] | None):
    if callable(x):
        reveal_type(x)  # `((...) -> Any) & (() -> object)` on `main`
                        # should be `((...) -> Any`
```

https://play.ty.dev/6f8a8a34-c7e9-41b5-b211-7701a3e905ad

We should fix this.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-23 20:03_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-10-23 20:03_

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-23 20:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-22 18:05_

---

_Closed by @charliermarsh on 2025-12-24 17:49_

---
