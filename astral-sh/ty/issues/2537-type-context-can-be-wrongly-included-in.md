```yaml
number: 2537
title: Type context can be wrongly included in diagnostic type when there is no solution
type: issue
state: open
author: randolf-scholz
labels:
  - diagnostics
  - generics
  - bidirectional inference
assignees: []
created_at: 2026-01-16T20:33:40Z
updated_at: 2026-01-17T04:20:39Z
url: https://github.com/astral-sh/ty/issues/2537
synced_at: 2026-01-17T05:15:22Z
```

# Type context can be wrongly included in diagnostic type when there is no solution

---

_@randolf-scholz_

### Summary

Consider a `dict[K, V]` like class with a get method:

```python
from typing import Any, reveal_type

class Map[K, V]:
    def set(self, key: K, value: V) -> None: ...
    def get[T=None](self, key: Any, default: T = None, /) -> V | T: ...

d: Map[str, int] = Map[str, int]()

# as expected, these are both `Any | None`
reveal_type(d.get("key"))  # int | None  ✅️
reveal_type(d.get("key", None))  # int | None  ✅️
# outer context should not overrule the argument type!
result: str = reveal_type(d.get("key", None))  # int | None | str ❌️
```

`d.get("key", None)` alone produces `int | None` as expected, but in the last case we have `str` as outer context. This gives an unsolvable situation for the type variable `T` as we would need both `T <: str` and `T :> None`. It seems `ty` gives priority to the outer constraint `T <: str` here, which seems misguided, as it produces an illogical `reveal_type` result: `int | None | str`.

But `d.get("key", None)` cannot produce a `str`!

Instead, the inner constraint `T :> None` should probably be prioritized in this situation.

See also mirror issues in `mypy` and `pyrefly` tracker: https://github.com/python/mypy/issues/20576, https://github.com/facebook/pyrefly/issues/2136






### Version

_No response_

---

_Comment by @carljm on 2026-01-17 01:45_

Just to be clear, this is purely an issue of the diagnostic / hover / revealed type -- there is no type checking correctness issue here?

I think this is a known issue with how we apply type context in generic cases. We would like to fix it, but we don't consider it super high priority since it just makes the revealed types sometimes wrongly include the context type, when there is no solution that satisfies the context type. It doesn't ever change the correctness of a program.

---

_Label `bidirectional inference` added by @carljm on 2026-01-17 01:45_

---

_Renamed from "Bad Type Variable resolution with outer context (`dict.get`)" to "Type context can be wrongly included in diagnostic type when there is no solution" by @carljm on 2026-01-17 01:45_

---

_Label `generics` added by @carljm on 2026-01-17 01:45_

---

_Comment by @carljm on 2026-01-17 01:46_

@ibraheemdev do we already have an issue open for this? If not, we can use this one.

---

_Label `diagnostics` added by @carljm on 2026-01-17 01:46_

---

_Comment by @electronick1 on 2026-01-17 02:20_

It looks like this PR https://github.com/astral-sh/ruff/pull/21267 is related, it adds context for cases when type can not be assigned.
[For example](https://github.com/astral-sh/ruff/pull/21267/files#diff-8c9ae4384d5808e43fce19d9835dd6e49048297bd4989c5379ebe0f476d34e4fR425) in annotations.md:
```
# error: [invalid-assignment] "Object of type `list[int | str]` is not assignable to `list[int]`"
g: list[int] = f("a")
```

`f("a")` - can not return `list[int | str]`

---

_Comment by @ibraheemdev on 2026-01-17 03:13_

> `f("a")` - can not return `list[int | str]`

I'm not sure what you mean by that. `f("a")` can and should produce a `list[int | str]` given a type context of `list[int | str]`. In this case, the constraints are unsolvable, and our attempt to solve them produces a `list[int | str]`. I do agree that it is slightly confusing, but it is not incorrect.

Ignoring the type context completely can lead to confusing diagnostics, which is why we removed that behavior in https://github.com/astral-sh/ruff/pull/21267. For example, we use the type context to avoid literal promotion
```py
from typing import Literal

def f[T](x: T) -> list[T]:
    return [x]

# Option A: Object of type `list[Literal["hello"] | int]` is not assignable to `list[Literal["hello"] | bool]`
# Option B: Object of type `list[Literal["hello"] | int]` is not assignable to `list[str | bool]`
x: list[Literal["hello"] | bool] = ["hello", 1]
```

Or to infer `TypedDict` types:
```py
class A(TypedDict):
    bar: int

# Option A: Object of type `list[A | int]` is not assignable to `list[A | bool]`
# Option B: Object of type `list[A | int | dict[Unknown | str, Unknown | int]]` is not assignable to `list[A | bool]`
y: list[A | bool] = [{"bar": 1}, 1]
```

The second diagnostic is a lot more confusing in both cases, and suggests that there is a problem with literal promotion or typed dict inference.

I do think we can be better in the generics case though, and should not resolve without the type context if we fail to arrive at a valid specialization.

---

_Comment by @electronick1 on 2026-01-17 04:03_

I mean, if we consider function:
```
def f[T](x: T) -> list[T]:
    return [x]
```

And function call `f("a")` - `f` will produce `list[str]`, while message says: "`list[int | str]` is not assignable to `list[int]`" - which is confusing in case of `f("a")` call, even if `T` of `f` could be any other type.

> I do think we can be better in the generics case though, and should not resolve without the type context if we fail to arrive at a valid specialization.

Yeah I was looking at specialization logic too, I can see your PR #22643 is addressing this 



---
