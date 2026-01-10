```yaml
number: 1420
title: "`invalid-return-type` on syntax `(A | None) and B` for return type `B | None`; needs narrowing"
type: issue
state: closed
author: CoderJoshDK
labels: []
assignees: []
created_at: 2025-10-23T15:00:37Z
updated_at: 2025-10-24T13:37:56Z
url: https://github.com/astral-sh/ty/issues/1420
synced_at: 2026-01-10T02:06:25Z
```

# `invalid-return-type` on syntax `(A | None) and B` for return type `B | None`; needs narrowing

---

_Issue opened by @CoderJoshDK on 2025-10-23 15:00_

### Summary

Valid python syntax is to do a union on boolean operations and return either the first or second component based on condition:

```py
a = "hit" and "this is the output"
b = "" and "misses this"
```

`ty` is unable to narrow the return type when mismatch types are present with a `~AlwaysTruthy`. To see this in action, https://play.ty.dev/9af3b976-3b09-4f7d-927e-570bcdd53260

```py
def foo(name: str) -> str | None:
    bar: dict[str, Foo] = {}
    baz = bar.get(name)
    return baz and f"This is string {baz}"
# Return type does not match returned value: expected `str | None`, found `(Foo & ~AlwaysTruthy) | None | str` (invalid-return-type)
```
The above code fails because `baz` is `Foo | None`. But this should be possible to narrow and say that `Foo` can never be returned. If `baz` is truthy, it returns the second component, the string. If `baz` is falsy, it returns None.

That said, technically `ty` is correct. This case is ambiguous;
Given the example:
```py
def foo(name: str) -> str | None:
    bar: dict[str, int] = {}
    baz = bar.get(name)
    return baz and f"This is string {baz}"
```
It is possible for it to return `0` instead of None. And that would be invalid based on the return type. However, if I modify the code to:
```py
def foo(name: str) -> str | None:
    bar: dict[str, int] = {}
    baz = bar.get(name)
    return (baz or None) and f"This is string {baz}"
# Return type does not match returned value: expected `str | None`, found `(int & ~AlwaysFalsy & ~AlwaysTruthy) | None | str`
```
This should no longer be possible to return `0`. And the "same" issue is present. Maybe you want to say that my first example is ambiguous and there is no way to narrow this down. But in this last example, `int` should never be possible to return. Narrowing should be possible. 

----

While I can find some issues that seem related to this. I feel as though this is unique and specific enough to warrant its own issue. More specifically, there are two things going on here. A) narrowing of truthy types and B) ambiguous cases here that you can or can not guarantee to be the case. 
Other type checkers are all over the place on this one. `Foo | None` case is valid in pyright, but not mypy. It is the reverse for `int | None` case. And both say my workaround for `(A or None)` is invalid. 
I don't expect `ty` to have parity with the other checkers. Just acknowledging that this is a complicated situation. 

### Version

ty 0.0.1-alpha.24 (1fee7da8b 2025-10-23)

---

_Comment by @AlexWaygood on 2025-10-24 09:59_

Thanks for the report! I had to fix some details before writing this up, so some of what I'm saying below only applies on `main` (though it will be included in the next release) -- in particular the bits about `__len__` and `@final` classes don't work correctly on the latest alpha 😄 But the playground is deployed from the `main` branch, so it should all apply if you try things out in the playground.

Unfortunately I think ty is behaving as expected here. Here's a slightly simpler version of your original example:

```py
class Foo: ...

def f(foo: Foo) -> None:
    # error: [invalid-return-type] "Return type does not match returned value: expected `None`, found `(Foo & ~AlwaysTruthy) | None`"
    return foo and None
```

At runtime, it looks like ty is behaving incorrectly here, because instances of _exactly_ `Foo` are always truthy!

```pycon
>>> class Foo: ...
... 
... def f(foo: Foo) -> None:
...     return foo and None
...     
>>> f(Foo()) is None
True
```

Unfortunately, however, `Foo` in a type annotation does _not_ mean "any instance of _exactly_ `Foo`"! It means "any instance of `Foo` _or_ a subclass of `Foo`". ty can't rule out here that there might be a subclass of `Foo` that would be sometimes or always falsy (because that's legal according to the type system), and it would also be perfectly legal according to the type system to pass an instance of such a subclass into the `f()` function here:

```pycon
>>> class FooSub(Foo):
...     def __bool__(self): return False
...     
>>> f(FooSub())
<__main__.FooSub object at 0x1055f8c20>
```

If ty could be confident that _all_ instances of `Foo` are always truthy, it would be able to simplify the type `Foo & ~AlwaysTruthy` to `Never` (the empty type), which would mean that the type `(Foo & ~AlwaysTruthy) | None` would simplify to `None`, which matches the return type -- therefore, in that situation, no `invalid-return-type` error would be emitted either on this simplified example or on your original snippet. But because of the possibility of subclasses, it cannot be confident that _all_ instances of `Foo` are always truthy, so it cannot make that simplification, so the diagnostic is emitted.

There _are_ situations in which ty can be confident that a subclass cannot change the truthiness of a base class. If a `__bool__` method is annotated as returning a `Literal` type, it's illegal according to the type system to change the return type of `__bool__` in a subclass override, so ty is allowed to assume that truthiness will be consistent for all subclasses, e.g.

```py
from typing import Literal

# ty knows that all instances of `A` will always be truthy
class A:
    def __bool__(self) -> Literal[True]:
        return True
```

If the class is marked as `@final` and `__len__` is defined, ty can also look at the return type of `__len__`: the runtime falls back to `__len__` to decide the truthiness of an object if `__bool__` is not defined. Generally it's not safe for ty to look at `__len__` because of the fact that `__bool__` always takes precedence, and could be defined on a subclass, but `@final` classes cannot be subclassed:

```py
from typing import Literal, final

# ty knows that all instances of `B` will always be falsy
@final
class B:
    def __len__(self) -> Literal[0]:
        return 0
```

And if a class defines neither `__len__` nor `__bool__`, but is marked `@final`, ty knows that it will always be truthy. Adding `@final` to the `Foo` class fixes the error in your first snippet.

```py
from typing import final

# ty knows that all instances of `C` will always be truthy
@final
class C: ...
```

---

_Closed by @AlexWaygood on 2025-10-24 09:59_

---
