```yaml
number: 1783
title: Type narrowing with typevars fails in certain paths
type: issue
state: closed
author: aidandj
labels: []
assignees: []
created_at: 2025-12-05T18:24:53Z
updated_at: 2025-12-05T20:04:07Z
url: https://github.com/astral-sh/ty/issues/1783
synced_at: 2026-01-12T15:54:25Z
```

# Type narrowing with typevars fails in certain paths

---

_@aidandj_

### Summary

When the `if` and `isinstance` checks are broken out there is no error, but when combined:

```
Return type does not match returned value: expected `B@get_t2 | None`, found `(Base & ~AlwaysTruthy) | None | B@get_t2` (invalid-return-type) [Ln 24, Col 16]
```

I searched through some of the existing type narrowing issues, but nothing stood out.

https://play.ty.dev/5cddf17c-a358-48e0-937e-6ca92c87ea66

```py
from typing import TypeVar, Type, NoReturn, Optional, Dict

class Base: ...

class Child(Base): ...

B = TypeVar("B", bound=Base)

class Thing(Dict[str, Base]):

    def get_t(self, key: str, t: Type[B]) -> Optional[B]:
        wf = self.get(key)
        if wf:
            if isinstance(wf, t):
                return wf
            else:
                raise TypeError("Invalid workflow type requested")
        return None

    def get_t2(self, key: str, t: Type[B]) -> Optional[B]:
        wf = self.get(key)
        if wf and not isinstance(wf, t):
            raise TypeError("Invalid workflow type requested")
        return wf

t = Thing()

c = t.get_t("c", Child)
c2 = t.get_t2("c", Child)
```

### Version

_No response_

---

_Comment by @carljm on 2025-12-05 19:14_

Thanks for the report!

I think this is a case where ty is actually (perhaps pedantically?) correct, but it would be ideal if the diagnostic could be more clear about why.

Nothing in your example rules out the possibility that instances of some subclass of `Base` could evaluate to `False` in a boolean context. If `wf` in `get_t2` were such an instance, it would fail `if wf`, short circuit, and reach the `return wf`, and would in fact be type-unsafe (because it would be an instance of `Base` that hasn't been verified to be an instance of `t`).

If you look at the error message, it says "expected `B@get_t2 | None`, found `(Base & ~AlwaysTruthy) | None | B@get_t2`". The problematic extra component of the returned union is `Base & ~AlwaysTruthy`. This type corresponds precisely to the above scenario: an instance of `Base` (or a subclass) which does not evaluate to truthy in boolean context.

There are two easy fixes for this in your code: you can [use `if wf is not None` instead of just `if wf`](https://play.ty.dev/7437c523-c268-4ef8-acfb-834f0ad3c16f), or you can [inform ty that instances of `Base` are always truthy](https://play.ty.dev/5c160892-0a93-4662-9b5b-ec0aaa492772). Either option eliminates the error.

The version of the code in `get_t` also eliminates the error, because that restructuring causes the "falsy instance of Base" scenario to instead fall through to a `return None`.

---

_Closed by @carljm on 2025-12-05 19:14_

---

_Comment by @carljm on 2025-12-05 19:20_

It is true that instances of `Base` itself will always be truthy, if it doesn't define `__bool__`. But there's no such guarantee for subclasses of `Base`. That means there's a third option here (which also eliminates the ty error): define `Base` as final. (Of course that doesn't work if you actually subclass it, as you do in this example.)

So ultimately this is another case where ty is pedantic about what is possible for subclasses to do. But in this particular case, mypy and pyrefly both also issue this error; only pyright does not. So I don't think we are being too pedantic here.

---

_Comment by @aidandj on 2025-12-05 20:04_

Thanks for the response, should have checked it against mypy as well.

---
