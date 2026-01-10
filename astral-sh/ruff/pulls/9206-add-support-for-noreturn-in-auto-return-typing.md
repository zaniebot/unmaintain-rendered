```yaml
number: 9206
title: "Add support for `NoReturn` in auto-return-typing"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/no-return
created_at: 2023-12-20T04:50:15Z
updated_at: 2023-12-22T06:57:18Z
url: https://github.com/astral-sh/ruff/pull/9206
synced_at: 2026-01-10T23:07:18Z
```

# Add support for `NoReturn` in auto-return-typing

---

_Pull request opened by @charliermarsh on 2023-12-20 04:50_

## Summary

Given a function like:

```python
def func(x: int):
    if not x:
        raise ValueError
    else:
        raise TypeError
```

We now correctly use `NoReturn` as the return type, rather than `None`.

Closes https://github.com/astral-sh/ruff/issues/9201.


---

_Renamed from "Add support for NoReturn in auto-return-typing" to "Add support for `NoReturn` in auto-return-typing" by @charliermarsh on 2023-12-20 04:53_

---

_Label `bug` added by @charliermarsh on 2023-12-20 04:53_

---

_Merged by @charliermarsh on 2023-12-20 05:06_

---

_Closed by @charliermarsh on 2023-12-20 05:06_

---

_Branch deleted on 2023-12-20 05:06_

---

_Comment by @github-actions[bot] on 2023-12-20 05:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @tooruu on 2023-12-20 13:46_

Does this use `typing.Never` in Python 3.11+?

---

_Comment by @max-muoto on 2023-12-20 21:31_

> Does this use `typing.Never` in Python 3.11+?

They're effectively identical in regards to type-checkers. Pyright (for Python 3.11+) [infers the correct return type for a function like the one given in the example as `NoReturn`](https://pyright-play.net/?code=CYUwZgBGCuB2DGAKAHgLggS1gFwJSoCgJjNJYB7bCNIkugJwEMMBnECANUYBtoQBRevXL1axENzaE6DZmwgAVAJ4AHAUJFA). `typing.Never` is used as a bottom type in situations like this:

```python
def int_or_str(arg: int | str) -> None:
    never_call_me(arg)  # type checker error
    match arg:
        case int():
            print("It's an int")
        case str():
            print("It's a str")
        case _: # type checkers considers this unreachable
            never_call_me(arg)  # Pyright would say `arg` is of type `Never`.
```

---

_Comment by @tooruu on 2023-12-22 06:53_

> > Does this use `typing.Never` in Python 3.11+?
> 
> They're effectively identical in regards to type-checkers. Pyright (for Python 3.11+) [infers the correct return type for a function like the one given in the example as `NoReturn`](https://pyright-play.net/?code=CYUwZgBGCuB2DGAKAHgLggS1gFwJSoCgJjNJYB7bCNIkugJwEMMBnECANUYBtoQBRevXL1axENzaE6DZmwgAVAJ4AHAUJFA). `typing.Never` is used as a bottom type in situations like this:
> 
> ```python
> def int_or_str(arg: int | str) -> None:
>     never_call_me(arg)  # type checker error
>     match arg:
>         case int():
>             print("It's an int")
>         case str():
>             print("It's a str")
>         case _: # type checkers considers this unreachable
>             never_call_me(arg)  # Pyright would say `arg` is of type `Never`.
> ```

[Official Python docs](https://docs.python.org/3/library/typing.html#typing.Never) state that starting in 3.11 `typing.Never` should be used instead of `typing.NoReturn`. Looks like this has been addressed by #9213.

---
