```yaml
number: 2018
title: Incorrect type inference
type: issue
state: closed
author: Yura52
labels: []
assignees: []
created_at: 2025-12-17T15:42:17Z
updated_at: 2025-12-17T18:32:12Z
url: https://github.com/astral-sh/ty/issues/2018
synced_at: 2026-01-10T01:53:59Z
```

# Incorrect type inference

---

_Issue opened by @Yura52 on 2025-12-17 15:42_

### Summary

(Please feel free to edit the issue title as needed!)

In the following snippet, `foo` passes the type checks, but `bar` does not:

```py
def foo[T](a: None | T, b: None | T) -> T:
    if a is None:
        assert b is not None
    else:
        b = a
    return b


def bar[T](a: None | T, b: None | T) -> T:
    if a is None:
        assert b is not None
    else:
        if b is None:
            b = a
    return b
```

Namely, `ty check` says:

```
error[invalid-return-type]: Return type does not match returned value
  --> local/ty_test.py:15:12
   |
13 |         if b is None:
14 |             b = a
15 |     return b
   |            ^ expected `T@bar`, found `None | T@bar`
   |
  ::: local/ty_test.py:9:41
   |
 9 | def bar[T](a: None | T, b: None | T) -> T:
   |                                         - Expected `T@bar` because of return type
10 |     if a is None:
11 |         assert b is not None
   |
info: rule `invalid-return-type` is enabled by default
```

However, `b` always becomes `T` by the end of both functions, so the reported error seems to be false positive.

### Version

0.0.2

---

_Comment by @oconnor663 on 2025-12-17 18:25_

It looks like we're failing to unify the two branches? https://play.ty.dev/9830b46c-09df-4123-a317-4e058ccc86e5

```py
def bar(a: int | None, b: int | None):
    if a is None:
        assert b is not None
        reveal_type(b)  # int
    else:
        if b is None:
            b = a
        reveal_type(b)  # int
    reveal_type(b)      # int | None
```

---

_Comment by @AlexWaygood on 2025-12-17 18:26_

It's possible this might be related to https://github.com/astral-sh/ty/issues/690

---

_Comment by @carljm on 2025-12-17 18:32_

Yes this is #690 - thanks for the report!

---

_Closed by @carljm on 2025-12-17 18:32_

---
