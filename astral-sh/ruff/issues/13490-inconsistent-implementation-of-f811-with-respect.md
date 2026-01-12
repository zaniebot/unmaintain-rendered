```yaml
number: 13490
title: Inconsistent implementation of F811 with respect to submodule imports
type: issue
state: open
author: AlexWaygood
labels:
  - rule
assignees: []
created_at: 2024-09-23T22:44:48Z
updated_at: 2024-09-23T23:28:11Z
url: https://github.com/astral-sh/ruff/issues/13490
synced_at: 2026-01-12T15:54:53Z
```

# Inconsistent implementation of F811 with respect to submodule imports

---

_@AlexWaygood_

Ruff [emits no F811 diagnostics for this snippet](https://play.ruff.rs/e2f74be7-eeff-4e61-ab90-867610850d2c):

```py
import ctypes
import ctypes.wintypes
```

However, it [emits an F811 diagnostic for this snippet](https://play.ruff.rs/31b4dec1-3302-4348-9ef9-de747700062b):

```py
import importlib

ctypes = importlib.import_module("ctypes")
import ctypes.wintypes
```

I think Ruff should either emit a diagnostic for both of these, or neither of these, since they're equivalent in terms of what happens with respect to symbol definitions:
1. A `ctypes` symbol is defined. For the first snippet this is defined via an `import` statement, and for the second it is defined via a function call, but this difference shouldn't really be relevant.
2. The `import ctypes.wintypes` statement causes:
   1. The `ctypes` variable to be reassigned in the global namespace (but to the same object, which is just looked up in the `sys.modules` cache)
   2. The `ctypes.wintypes` submodule to be loaded and stored as a `wintypes` attribute on the `ctypes` module object

I would lean towards _not_ emitting a diagnostic on either of these, because although the submodule import of `ctypes.wintypes` is strictly-speaking a redefinition of the `ctypes` variable in both cases, many people consider it unidiomatic to access top-level members from `ctypes` in a module that only has `import ctypes.wintypes` in it. (It's sort-of an "implicit import" of the top-level `ctypes` module.)

To be clear: I'm not suggesting special-casing the `importlib.import_module`. I'm suggesting that a submodule `foo.bar` import should be considered a redefinition of a prior `foo.bar` import, and should never be considered a redefinition of a previously defined `foo` symbol.

This came up in https://github.com/python/cpython/pull/124384.

Terms I searched for before filing the issue:
- F811
- redefined-while-unused

---

_Label `rule` added by @AlexWaygood on 2024-09-23 22:44_

---

_Comment by @zanieb on 2024-09-23 22:48_

I agree we should probably not emit in the second case.

---
