---
number: 1257
title: "Incomplete assignability implementation between two `Callable` types where one `Callable` type has `*args: Any, **kwargs: Any`"
type: issue
state: open
author: AlexWaygood
labels:
  - calls
  - type properties
assignees: []
created_at: 2025-09-25T15:54:03Z
updated_at: 2026-01-09T02:58:46Z
url: https://github.com/astral-sh/ty/issues/1257
synced_at: 2026-01-10T01:48:23Z
---

# Incomplete assignability implementation between two `Callable` types where one `Callable` type has `*args: Any, **kwargs: Any`

---

_Issue opened by @AlexWaygood on 2025-09-25 15:54_

### Summary

The typing spec [states](https://typing.python.org/en/latest/spec/callables.html#meaning-of-in-callable):

> If the input signature in a function definition includes both a `*args` and `**kwargs` parameter and both are typed as `Any` (explicitly or implicitly because it has no annotation), a type checker should treat this as the equivalent of `...`. Any other parameters in the signature are unaffected and are retained as part of the signature

And we can see that in the conformance test suite, [this test](https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/callables_annotation.py#L157) asserts that `Proto7` should be understood as being assignable to `Proto6`: although the signature of `Proto7` is less permissive than the signature of `Proto6`, `Proto6` has a gradual signature due to the presence of `*args: Any` and `**kwargs: Any` in the signature, so the normal rules do not apply. The definitions of `Proto6` and `Proto7` in this test are:

```py
from typing import Protocol, Any

class Proto6(Protocol):
    def __call__(self, a: int, /, *args: Any, k: str, **kwargs: Any) -> None: ...

class Proto7(Protocol):
    def __call__(self, a: float, /, b: int, *, k: str, m: str) -> None: ...
```

Note that both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=7356c7d052605729aa815aa7c84ceea4) and [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0AFag1wBjXFAA0dAILpSAHXRLRUVHDiDhuAGwAKIbhHioASkRK6Vuphhg6AfQejUUKE71wYUMNNSJOdAZpAHppACpUSjY4ALlSaQBrALgGSgjwxIB3KJi4%2BVM6AFoAPjoAOVx0GADCOqUVNQ0tI1wAdgNtE3NLa1t7Jxc3Dy8fPwCwKFxUYLowumwA1lnwpJS06Rp1ykLSiqqaujrCBqw7OjA9YjaAwxE2yR70azpcRIBGd9vtHToAXjo1ysAGI6AB5ADSShAkhAAFcGNA4CRyIgQKCAKqIqAQJgXOHoUSIqpwU79C68GgzBzoOE0bAwSh6fBLIK7MqpSgWZ7WSgwBhwyjPMAKEDlOkMrl0YD4AC%2BouhsthqCJEAAbjAAGLQGAUNBYPBEMggWVAA&version=3.12) also appear to fail this test currently; only [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAK4MYAxmADYA0UAginALABQLpFAhgM5eHFgA2ABREwJchQCUALhZR5UACYBTYFAD660hwoVNQrsorAaHaZhQwaAehoAqDiDRdz9ODQDW5rjBD27HgDujs6uDJJQALQAfFAAcmAoyuYAdGksbJw8fGJgAOwi-BIycgoqaprauvqGxqbmwBRgHFZQtlAARuaorXae3r40EAMgETHxiclQaSkZzOVQwEIIeeaiJHklzApQYB4AjPtr-AJQALxQK-IAxFAA8gDSLEA) currently passes this test.

A version of this failing test for ty that doesn't involve protocols would be:

```py
from typing import Any
from ty_extensions import CallableTypeOf, is_assignable_to, static_assert

def f(*args: Any, a: int, **kwargs: Any): ...
def g(fwomp: int, /, a: int, *, bar: str, baz: str): ...

static_assert(is_assignable_to(CallableTypeOf[g], CallableTypeOf[f]))
```

https://play.ty.dev/4f422ed6-1725-4230-b803-a62325e218f6

### Version

_No response_

---

_Label `calls` added by @AlexWaygood on 2025-09-25 15:54_

---

_Label `type properties` added by @AlexWaygood on 2025-09-25 15:54_

---

_Comment by @carljm on 2025-09-25 16:06_

I think our current implementation only treats a function as having a "gradual signature" if it has `*args: Any, **kwargs: Any` as its _only_ parameters. I didn't actually realize that functions with `*args: Any, **kwargs: Any` and other parameters as well were also supposed to be treated specially.

---

_Comment by @AlexWaygood on 2025-09-25 16:07_

> I didn't actually realize that functions with `*args: Any, **kwargs: Any` and other parameters as well were also supposed to be treated specially.

No, neither did I. It's quite surprising to me TBH, but it's what the spec says, and it's what the conformance test suite enforces (the above test starts failing on https://github.com/astral-sh/ruff/pull/20368)

---

_Comment by @erictraut on 2025-09-25 17:11_

> It's quite surprising to me TBH

When we were working on that part of the spec, we wanted to ensure that any callable signature that could be described using the `Callable` special form could also be described using a callback protocol. With the `Callable` special form, it's possible to express `Callable[Concatenate[int, ...], None]` which describes a callable with a position-only `int` parameter plus a gradual form for additional parameters. If you want to express the same signature using a callback protocol, you can use `def __call__(self, x: int, /, *args: Any, **kwargs: Any) -> None: ...`. 

---

_Comment by @AlexWaygood on 2025-09-25 17:22_

Thanks @erictraut, that makes a lot of sense!

---

_Added to milestone `Stable` by @carljm on 2026-01-09 02:58_

---
