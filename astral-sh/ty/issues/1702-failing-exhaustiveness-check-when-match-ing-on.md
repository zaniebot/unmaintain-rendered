```yaml
number: 1702
title: "Failing exhaustiveness check when `match`ing on generic classes"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - control flow
assignees: []
created_at: 2025-12-01T11:37:11Z
updated_at: 2025-12-02T07:16:44Z
url: https://github.com/astral-sh/ty/issues/1702
synced_at: 2026-01-10T01:58:59Z
```

# Failing exhaustiveness check when `match`ing on generic classes

---

_Issue opened by @sharkdp on 2025-12-01 11:37_

> This issue still persists when matching against classes that use generics: 
> 
> ```python
> class Ok[T]:
>     def __init__(self, value: T) -> None:
>         self._value = value
> 
> 
> class Err[E]:
>     def __init__(self, value: E) -> None:
>         self._value = value
> 
> 
> def get_data() -> Ok[str] | Err[str]:
>     return Ok("example")
> 
> 
> def func() -> str:
>     match get_data():
>         case Ok():
>             return "Foo"
>         case Err():
>             return "Bar"
> ```
> 
> Function can implicitly return `None`, which is not assignable to return type `str`
>  

 _Originally posted by @sasanjac in [#99](https://github.com/astral-sh/ty/issues/99#issuecomment-3596000374)_

---

_Label `bug` added by @sharkdp on 2025-12-01 11:37_

---

_Label `control flow` added by @sharkdp on 2025-12-01 11:37_

---

_Comment by @AlexWaygood on 2025-12-01 11:40_

Here's an adapted repro, that shows that we do infer the type as `Never` after the `Ok()` and `Err()` branches:

```py
class Ok[T]:
    def __init__(self, value: T) -> None:
        self._value = value


class Err[E]:
    def __init__(self, value: E) -> None:
        self._value = value


# error: [invalid-return-type] "Function can implicitly return `None`"
def func(x: Ok[str] | Err[str]) -> str:
    match x:
        case Ok():
            reveal_type(x)  # revealed: Ok[str] | (Err[str] & Ok[object])
            return "Foo"
        case Err():
            reveal_type(x)  # revealed: Err[str] & ~Ok[object]
            return "Bar"
        case _:
            reveal_type(x)  # revealed: Never
```

---

_Comment by @sharkdp on 2025-12-01 11:47_

Narrowing and reachability analysis are two different kinds of things, unfortunately. Narrowing works, exhaustiveness checking does not. That's why I labeled as `control flow` instead of `narrowing`. I think I know what the problem is. We'll need to use the top materialization when creating the narrowed subject type. PR incoming.


---

_Comment by @AlexWaygood on 2025-12-01 11:54_

> Narrowing and reachability analysis are two different kinds of things, unfortunately. Narrowing works, exhaustiveness checking does not. That's why I labeled as `control flow` instead of `narrowing`.

yes -- I just wanted to clarify that the issue was limited to control flow, and that there was no similar issue w.r.t. narrowing :-)

---

_Closed by @sharkdp on 2025-12-01 12:52_

---

_Comment by @sasanjac on 2025-12-02 06:56_

Yes, the most obvious workaround here its to use


```python
case _:
    t.assert_never(arg)
```

since narrowing works, however  it does not make sense in all cases, e.g. in my example since the Result union won't get extended by definition.

---

_Comment by @sharkdp on 2025-12-02 07:16_

@sasanjac The bug should have been fixed by https://github.com/astral-sh/ruff/pull/21726

---
