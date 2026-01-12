```yaml
number: 2448
title: Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument
type: issue
state: open
author: hyperkai
labels: []
assignees: []
created_at: 2026-01-11T17:06:23Z
updated_at: 2026-01-11T17:37:04Z
url: https://github.com/astral-sh/ty/issues/2448
synced_at: 2026-01-12T02:26:12Z
```

# Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument

---

_Issue opened by @hyperkai on 2026-01-11 17:06_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0

A generic type argument accepts a tuple as shown below:

```python
       # ↓↓↓↓↓↓
v1: list[(int,)]
v1 = [0, 1, 2]       # No error
v1 = ['A', 'B', 'C'] # Error

       # ↓↓↓↓↓↓
v2: list[(str,)]
v2 = [0, 1, 2]       # Error
v2 = ['A', 'B', 'C'] # No error
```

```python
        # ↓↓↓↓↓↓↓↓↓↓
v1: tuple[(int, ...)]
v1 = (0, 1, 2)       # No error
v1 = ('A', 'B', 'C') # Error

        # ↓↓↓↓↓↓↓↓↓↓
v2: tuple[(str, ...)]
v2 = (0, 1, 2)       # Error
v2 = ('A', 'B', 'C') # No error
```

### Version

_No response_

---

_Comment by @AlexWaygood on 2026-01-11 17:36_

`tuple[(int, ...)]` and `tuple[(str, ...)]` seem fine to me, since they have almost exactly the same AST as `tuple[int, ...]` and `tuple[str, ...]` (respectively). I agree we should emit diagnostics for `list[(int,)]` and `list[(str,)]`, though.

---

_Renamed from "A generic type argument accepts a tuple" to "Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument" by @AlexWaygood on 2026-01-11 17:37_

---
