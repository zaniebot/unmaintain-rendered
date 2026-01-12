```yaml
number: 2448
title: Emit diagnostics for generic class parameterized with a single-element tuple of types if the generic class accepts only a single type argument
type: issue
state: open
author: hyperkai
labels:
  - needs-info
assignees: []
created_at: 2026-01-11T17:06:23Z
updated_at: 2026-01-12T06:14:32Z
url: https://github.com/astral-sh/ty/issues/2448
synced_at: 2026-01-12T06:54:49Z
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

_Label `diagnostics` added by @dhruvmanila on 2026-01-12 05:57_

---

_Comment by @dhruvmanila on 2026-01-12 06:13_

> I agree we should emit diagnostics for `list[(int,)]` and `list[(str,)]`, though.

I don't think we should emit a diagnostic here as this is a valid specialization because a tuple of types gets matched against individual type parameters in the generic class in the same way as `list[int, str]` (which is a tuple in the AST) or `list[(int, str)]` does. None of the other type checkers emit a diagnostic either.

@hyperkai Can you clarify what issue are you facing? Are you expecting that ty should raise an error where you've mentioned "No error" as a comment? Or is it something else?

---

_Label `diagnostics` removed by @dhruvmanila on 2026-01-12 06:13_

---

_Label `needs-info` added by @dhruvmanila on 2026-01-12 06:14_

---
