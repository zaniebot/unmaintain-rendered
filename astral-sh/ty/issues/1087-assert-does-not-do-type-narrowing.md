```yaml
number: 1087
title: assert does not do type narrowing
type: issue
state: closed
author: bugnano
labels:
  - generics
assignees: []
created_at: 2025-08-22T06:28:49Z
updated_at: 2025-08-22T06:46:33Z
url: https://github.com/astral-sh/ty/issues/1087
synced_at: 2026-01-12T15:54:24Z
```

# assert does not do type narrowing

---

_@bugnano_

### Summary

If there's a function like this:

```python
def my_function() -> str | None:
    return "hello"
```

in some cases I know that the function does not return `None`.

When using mypy, I can write a function like so:

```python
def not_none[T](obj: T | None) -> T:
    assert obj is not None
    return obj
```

and if I use it

```python
hi = not_none(my_function()).upper()
```

mypy correctly understands that the retrun value of `not_none` is not None, but ty errors out stating that I cannot invoke `upper()` on None.

### Version

ty 0.0.1-alpha.19

---

_Label `generics` added by @sharkdp on 2025-08-22 06:44_

---

_Comment by @sharkdp on 2025-08-22 06:46_

Thank you for reporting this.

Type narrowing on `assert` statements does work. The problem here is that our generics solver is not yet capable enough to solve `T` as `str` here. Instead, it solves `T` as `str | None`, which is also a solution, but obviously not the intended/best one. Improving this is being tracked in https://github.com/astral-sh/ty/issues/623.

The non-generic version of this code works just fine: https://play.ty.dev/074988a3-daf8-43ae-93fc-d14c92bf93e0 (notice that we would correctly error on the `return obj` statement if the `assert` statement was not there).

---

_Closed by @sharkdp on 2025-08-22 06:46_

---
