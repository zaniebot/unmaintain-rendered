```yaml
number: 459
title: Daily property test run failed on Tue May 20 2025
type: issue
state: closed
author: github-actions
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-05-20T12:05:35Z
updated_at: 2025-05-20T18:11:04Z
url: https://github.com/astral-sh/ty/issues/459
synced_at: 2026-01-12T15:54:23Z
```

# Daily property test run failed on Tue May 20 2025

---

_@github-actions_

Run listed here: https://github.com/astral-sh/ty/actions/runs/15136976142

---

_Label `bug` added by @github-actions[bot] on 2025-05-20 12:05_

---

_Label `type properties` added by @github-actions[bot] on 2025-05-20 12:05_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-20 14:36_

---

_Comment by @AlexWaygood on 2025-05-20 15:47_

Minimal repro:

```py
from ty_extensions import static_assert, is_equivalent_to, TypeOf

static_assert(is_equivalent_to(TypeOf[int.bit_length], TypeOf[int.bit_length]))
```

The assertion now fails on `main`

The definition for `int.bit_length()` is about as simple as it gets:

https://github.com/astral-sh/ruff/blob/60b486abce4e756fbd26a586a03d551ba44b9ea8/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L253

---

_Comment by @AlexWaygood on 2025-05-20 15:52_

(Confirmed that this passed before https://github.com/astral-sh/ruff/commit/97058e80930969d63bc2cdc6fcff8f84a2d7de4e but fails on that commit)

---

_Comment by @AlexWaygood on 2025-05-20 15:55_

This also fails, which I think confirms that we do not consider the type equivalent to itself anymore:

```py
type X = TypeOf[int.bit_length]

static_assert(is_equivalent_to(X, X))
```

---

_Comment by @AlexWaygood on 2025-05-20 16:00_

Even simpler:

```py
from ty_extensions import is_equivalent_to, TypeOf

def f(): ...

reveal_type(is_equivalent_to(TypeOf[f], TypeOf[f]))  # revealed: Literal[False] ??
```

---

_Comment by @dcreager on 2025-05-20 16:17_

@carljm, @AlexWaygood and I just discussed this at the PyCon sprint. The issue is that as of https://github.com/astral-sh/ruff/pull/18155, we take the parameter/return type annotations into account when comparing function literals for subtyping/equivalence. If an annotation is not given, we fall back on `Unknown`, which is gradual. But I did not also update `is_fully_static` to take this into account. So the subtyping/equivalence checks would return `False` when encountering the unannotated parameter/return in.

For now we are going to fix the glitch by updating `is_fully_static` to be consistent with the other methods. A longer term fix is to have separate representations for function literals and specialized functions, similar to the distinction we already make between `ClassLiteral` and `ClassType`.

---

_Closed by @AlexWaygood on 2025-05-20 18:11_

---
