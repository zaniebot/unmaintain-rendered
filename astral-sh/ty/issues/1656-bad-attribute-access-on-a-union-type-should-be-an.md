---
number: 1656
title: Bad attribute access on a union type should be an error, not a warning
type: issue
state: open
author: AlexWaygood
labels:
  - diagnostics
  - attribute access
assignees: []
created_at: 2025-11-27T13:12:44Z
updated_at: 2026-01-09T15:38:53Z
url: https://github.com/astral-sh/ty/issues/1656
synced_at: 2026-01-10T01:48:23Z
---

# Bad attribute access on a union type should be an error, not a warning

---

_Issue opened by @AlexWaygood on 2025-11-27 13:12_

### Summary

Consider the following examples:

```py
def f(x: list[int] | None, y: None):
    x[0]  # error: [non-subscriptable]
    y[0]  # error: [non-subscriptable]

    for z in x: ...  # error: [not-iterable]
    for z in y: ...  # error: [not-iterable]

    x.index  # warning: [possibly-missing-attribute]
    y.index  # error: [unresolved-attribute]
```

When it comes to subscripting a union or iterating over a union, we issue a hard error if any element of the union does not support the operation. That's as it should be: these are serious typing errors where we're confident in our analysis. The user should definitely want these errors surfaced even if they didn't want any warnings displayed.

When it comes to attribute access on a union, however, we only emit a `possibly-missing-attribute` _warning_ if some union elements do not have the attribute, rather than an `unresolved-attribute` error. I don't think this is correct; it's just as serious a typing error as attempting to subscript a union where some elements don't support subscription.

Ideally we would be able to distinguish attribute access on a union from something like this, which I think _should_ remain a warning rather than an error:

```py
def coinflip() -> bool:
    return False

class F:
    if coinflip():
        x = 42

F.x  # error: [possibly-missing-attribute]
```

Currently our member-lookup machinery conflates these cases

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-27 13:12_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-27 13:12_

---

_Label `attribute access` added by @AlexWaygood on 2025-11-27 13:12_

---

_Removed from milestone `Stable` by @carljm on 2025-12-29 19:25_

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-29 19:25_

---

_Comment by @carljm on 2026-01-09 02:14_

https://github.com/astral-sh/ty/issues/940 is the same thing, but for calls

---

_Assigned to @carljm by @carljm on 2026-01-09 15:38_

---
