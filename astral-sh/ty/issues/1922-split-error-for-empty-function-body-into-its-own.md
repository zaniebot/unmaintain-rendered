```yaml
number: 1922
title: Split error for empty function body into its own error code
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-12-16T12:30:48Z
updated_at: 2025-12-16T14:30:04Z
url: https://github.com/astral-sh/ty/issues/1922
synced_at: 2026-01-12T15:54:26Z
```

# Split error for empty function body into its own error code

---

_@AlexWaygood_

Pyright allows functions to unsoundly have empty bodies even if they have a non-`None` return type, and mypy used to allow this too. As a result, we've seen several instances of users being confused by the fact that we do not allow this. For example:

```py
def foo() -> int: ...  # error[invalid-return-type]
```

It's a deliberate decision from us to be more strict than pyright here, since this function obviously does not return an `int` at runtime. We already have a [subdiagnostic](https://github.com/astral-sh/ruff/blob/5b1d3ac9b9c458448e388075945869a4d49296a9/crates/ty_python_semantic/src/types/diagnostic.rs#L2676-L2679) message that explains the specific conditions in which we do allow functions to have empty bodies. However, splitting this into a separate `empty-body` error code would allow users who are heavily impacted by this inconsistency to switch it off on their codebase (even just temporarily while they work down the number of errors). This would also match mypy's behaviour (mypy has a dedicated `empty-body` error code.



---

_Label `diagnostics` added by @AlexWaygood on 2025-12-16 12:30_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 14:30_

---
