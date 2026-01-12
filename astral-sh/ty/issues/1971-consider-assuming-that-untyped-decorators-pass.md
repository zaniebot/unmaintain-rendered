```yaml
number: 1971
title: Consider assuming that untyped decorators pass through the signature unchanged
type: issue
state: open
author: afonsotrepa
labels:
  - needs-decision
  - calls
  - type-inference
assignees: []
created_at: 2025-12-17T02:19:44Z
updated_at: 2025-12-18T23:02:56Z
url: https://github.com/astral-sh/ty/issues/1971
synced_at: 2026-01-12T15:54:26Z
```

# Consider assuming that untyped decorators pass through the signature unchanged

---

_@afonsotrepa_

### Summary

I ran into this problem where ty wouldn't catch a mistyped return type but pyright would. Narrowed it down to a decorator I'm using.
Sorry if this is a known issue (a quick search didn't find another open issue for this) or limitation.

### Example
```Python
def deco(_):
    def inner(func):
        return func
    return inner


@deco(0)
def f() -> str:
    return ""


def g() -> float:
    return f()
```

Expected to get "error[invalid-return-type]" (which is indeed what we see if we remove the decorator) but instead got all checks passing.


### Version

0.0.2


P.S.: Still loving ty and ruff, keep up the good work!

---

_Renamed from "ty fails to infer return type through untyped decorator factories" to "Failure to infer return type through untyped decorator factory" by @afonsotrepa on 2025-12-17 02:22_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-17 08:48_

---

_Comment by @sharkdp on 2025-12-17 08:53_

Thank you for reporting this.

We currently don't support return type inference yet (which is why the return type of `deco` is just `Unknown`), but plan to add it soon. See #128 for details. 

Note: It's actually not completely clear to me how other type checkers handle this case. It seems to me like we would have to infer a generic `∀ T. Callable[[T], T]` type for `inner` here (and, in effect, for the return type of `deco`)? But pyright simply infers the type of `deco` as `(_: Unknown) -> ((func: Unknown) -> Unknown)`. So it looks like it might use additional heuristics (something like: "if the return type of a decorator is Unknown, just pretend it acts like an identity operation")?

---

_Label `generics` added by @sharkdp on 2025-12-17 08:54_

---

_Label `calls` added by @sharkdp on 2025-12-17 08:54_

---

_Label `type-inference` added by @sharkdp on 2025-12-17 08:54_

---

_Comment by @carljm on 2025-12-18 00:30_

Yes, I think I remember some discussion of this. At least pyright (maybe some other type checkers too) do make the assumption that an untyped decorator doesn't change the signature. I'm not sold on this idea, though I can see why it's attractive.

---

_Comment by @carljm on 2025-12-18 00:31_

I'm not sure this is really blocked by #128, since I don't think #128 will solve it, nor is necessary to "solve" it in the way that other type checkers do.

---

_Renamed from "Failure to infer return type through untyped decorator factory" to "Consider assuming that untyped decorators pass through the signature unchanged" by @carljm on 2025-12-18 00:33_

---

_Label `needs-decision` added by @carljm on 2025-12-18 00:33_

---

_Label `generics` removed by @carljm on 2025-12-18 00:33_

---

_Comment by @afonsotrepa on 2025-12-18 01:34_

Actually this issue is still present if we type hint the decorator (and this was my original issue, I was just trying to create a minimalist example but clearly cut too much out, sorry):

```Python
from collections.abc import Callable

def deco[**P, R](_: object) -> Callable[[Callable[P, R]], Callable[P, R]]:
    def inner(func: Callable[P, R]) -> Callable[P, R]:
        return func
    return inner


@deco(0)
def f() -> str:
    return ""


def g() -> float:
    return f()
```

Same deal, ty (0.0.2) says all checks passed while pyright (1.1.407) correctly detects a "reportReturnType" (Type "str" is not assignable to return type "float")  error.

---

_Comment by @carljm on 2025-12-18 15:48_

@afonsotrepa Thanks for the example! It looks like that is a different limitation: https://github.com/astral-sh/ty/issues/1136

---

_Comment by @carljm on 2025-12-18 15:58_

@afonsotrepa note also that this version works in all type checkers (including ty) today, because it doesn't rely on the special-cased heuristic in #1136:

```py
from typing import Callable, Protocol

class Identity(Protocol):
    def __call__[**P, R](self, fn: Callable[P, R], /) -> Callable[P, R]: ...

def deco(_: object) -> Identity:
    def inner[**P, R](func: Callable[P, R]) -> Callable[P, R]:
        return func
    return inner

@deco(0)
def f() -> str:
    return ""


def g() -> float:
    return f()
```

---

_Comment by @afonsotrepa on 2025-12-18 17:19_

@carljm your Identity class fixed my issue, thanks!

---

_Comment by @carljm on 2025-12-18 21:53_

We could also do this in a way that is LSP-specific, by implementing some form of "best-guess types" that we won't use to emit type diagnostics, but will use for LSP purposes.

---

_Comment by @afonsotrepa on 2025-12-18 23:02_

@carljm sorry for another "off-topic" comment but I found your Identity class was not working for generic functions when using functools.wraps, so instead I'm now using:

```Python
def deco[**P, R](_: object) -> Callable[[Callable[P, R]], Callable[P, R]]:

    def dec(func: Callable[P, R]) -> Callable[P, R]:

        @functools.wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
                return func(*args, **kwargs)

        return wrapper

    return dec
```

This works with ty and pyright and successfully catches invalid return types, even for functions with generics. Hopefully this will be useful for someone else too.

---
