```yaml
number: 616
title: type of fractional powers of negative integers inferred as intergers
type: issue
state: closed
author: MatthewMckee4
labels: []
assignees: []
created_at: 2025-06-09T13:47:56Z
updated_at: 2025-11-12T02:00:43Z
url: https://github.com/astral-sh/ty/issues/616
synced_at: 2026-01-10T02:06:24Z
```

# type of fractional powers of negative integers inferred as intergers

---

_Issue opened by @MatthewMckee4 on 2025-06-09 13:47_

### Summary

mypy and pyright both support inferring this:
```py
b = (-1) ** (1 / 2)

reveal_type(b) # revealed: complex
```
but we currently infer as int

---

_Comment by @AlexWaygood on 2025-06-09 14:01_

Thanks. The right-hand side here is evaluated by ty as being a value of type `float`, so a simpler repro is:

```py
b = (-1) ** 0.5
reveal_type(b)
```

As with your repro, mypy and pyright both infer this correctly as `complex`, but ty infers it incorrectly as `int`.

The reason for this is because of [the overloads for `float.__rpow__`](https://github.com/python/typeshed/blob/8f5a80e7b741226e525bd90e45496b8f68dc69b6/stdlib/builtins.pyi#L385-L391), which is the method that ends up being called to evaluate this operation. The first two overloads both use type aliases for their annotations. Unfortunately, we don't support old-style type aliases yet, so we end up inferring both aliases as `Todo`, which is assignable to anything. That means that we incorrectly pick the first `float.__rpow__` overload when we should be picking the second -- because we (incorrectly) think all three overloads are possible matches, we just pick the first one.

So this should get naturally fixed as soon as we support old-style type aliases.

---

_Closed by @AlexWaygood on 2025-06-09 14:01_

---

_Comment by @MatthewMckee4 on 2025-06-09 14:02_

Ah, thank you

---

_Comment by @carljm on 2025-11-12 02:00_

Confirmed this will be fixed by https://github.com/astral-sh/ruff/pull/21394

---
