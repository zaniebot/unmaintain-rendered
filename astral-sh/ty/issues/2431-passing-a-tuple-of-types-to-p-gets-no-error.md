```yaml
number: 2431
title: "Passing a tuple of types to `**P` gets no error against error message"
type: issue
state: closed
author: hyperkai
labels:
  - diagnostics
assignees: []
created_at: 2026-01-10T00:19:36Z
updated_at: 2026-01-12T08:16:56Z
url: https://github.com/astral-sh/ty/issues/2431
synced_at: 2026-01-12T15:54:26Z
```

# Passing a tuple of types to `**P` gets no error against error message

---

_@hyperkai_

### Summary

*Memo:
- ty check
- ty 0.0.7
- Python 3.14.0
- Windows 11

Passing a set of types to `**P` gets the error message which doesn't say **a tuple of types** as shown below:

```python
from collections.abc import Callable

type TA[**P] = Callable[P, None]

    # ↓↓↓↓↓↓↓↓↓↓
v: TA[{int, str}] # Error
```

> error[invalid-type-arguments]: Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`

But passing a tuple of types to `**P` gets no error against the error message as shown below:

```python
from collections.abc import Callable

type TA[**P] = Callable[P, None]

    # ↓↓↓↓↓↓↓↓↓↓
v: TA[(int, str)] # No error
```

So, the error message should be like below:

> error[invalid-type-arguments]: Type argument for `ParamSpec` must be either a list or tuple of types, `ParamSpec`, `Concatenate`, or `...`


### Version

_No response_

---

_Comment by @carljm on 2026-01-10 00:23_

Thanks for the report! We don't support TypeVarTuple yet. While we are still in beta, it will be useful to refer to #1889 when filing issues.

---

_Closed by @carljm on 2026-01-10 00:23_

---

_Comment by @hyperkai on 2026-01-10 00:24_

No, it's `ParamSpec` but not `TypeVarTuple`.

---

_Comment by @carljm on 2026-01-10 00:28_

Oh! Sorry, reading too quickly.

---

_Reopened by @carljm on 2026-01-10 00:28_

---

_Label `overloads` added by @carljm on 2026-01-10 00:29_

---

_Label `diagnostics` added by @carljm on 2026-01-10 00:29_

---

_Comment by @carljm on 2026-01-10 00:40_

This is a bit subtle. The [typing spec](https://typing.python.org/en/latest/spec/generics.html#user-defined-generic-classes) says that for "aesthetic purposes", as a special case, when a class is parametrized by just one ParamSpec, it is valid to specialize it without extra brackets, e.g. `TA[int, str]` instead of `TA[[int, str]]`. But in the Python grammar and AST, `TA[int, str]` is indistinguishable from `TA[(int, str)]`, since the tuple literal (other than empty tuple literal) is defined by the comma, parentheses are just for grouping clarity. So the fact that we support `TA[(int, str)]` is just an unavoidable side effect of supporting `TA[int, str]` as required by spec.

But we do not support tuples to specialize ParamSpec in the general case (and neither do other type checkers):

```py
from collections.abc import Callable

type TA[**P, T] = Callable[P, None]

v: TA[(int, str), int] # this is an error!
```

So it would not be accurate to change the diagnostic message as suggested.

I don't think we should attempt to fully explain the special case for single-paramspec-only in our diagnostic message here, so I'm not sure there's any action needed.

---

_Closed by @carljm on 2026-01-10 00:40_

---

_Label `overloads` removed by @dhruvmanila on 2026-01-12 08:16_

---
