```yaml
number: 2565
title: "`type(x) is t` doesn't work for `t: type[T]`"
type: issue
state: open
author: arjenzorgdoc
labels:
  - narrowing
assignees: []
created_at: 2026-01-19T16:26:12Z
updated_at: 2026-01-19T16:33:08Z
url: https://github.com/astral-sh/ty/issues/2565
synced_at: 2026-01-19T17:25:58Z
```

# `type(x) is t` doesn't work for `t: type[T]`

---

_@arjenzorgdoc_

### Summary

`type(x) is y` doesn't work if `y` is of type `type[T]`. It does work with `isinstance`

I think this should all type check, but ty thinks `bad` is not ok:

```python
def good1(b: object) -> int:
    if type(b) is int:
        return b
    raise NotImplementedError


def good2[T: int](b: object, t: type[T]) -> int:
    if isinstance(b, t):
        return b
    raise NotImplementedError


def bad[T: int](b: object, t: type[T]) -> int:
    if type(b) is t:
        return b   # < ---- type error here
    raise NotImplementedError
```

https://play.ty.dev/12096b28-8338-4038-a5c5-5db362115bd6

My workaround:
```python
    if type(b) is t and isinstance(b, t):
        return b
```

### Version

55a174ed9 2026-01-19

---

_Label `narrowing` added by @AlexWaygood on 2026-01-19 16:29_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-19 16:30_

---

_Comment by @AlexWaygood on 2026-01-19 16:33_

thanks! I agree that narrowing here would be sound, as long as `T` does not have a generic class as its upper bound.

`if x is list[int]` is not a valid narrowing operation, so I don't think we could narrow for something like this:

```py
def bad2[T: list[int]](b: object, t: type[T]) -> list[int]:
    if type(b) is t:
        return b
    raise NotImplementedError
```

But narrowing seems fine if it's bound to a non-generic class, as in your example.

---
