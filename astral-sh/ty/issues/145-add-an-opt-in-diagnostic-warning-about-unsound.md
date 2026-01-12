```yaml
number: 145
title: "Add an opt-in diagnostic warning about unsound calls to `type[]`"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-03-31T11:37:20Z
updated_at: 2025-05-11T07:57:41Z
url: https://github.com/astral-sh/ty/issues/145
synced_at: 2026-01-12T15:54:22Z
```

# Add an opt-in diagnostic warning about unsound calls to `type[]`

---

_@AlexWaygood_

Red-knot allows you to call `type[]` types, and accurately infers the result of these calls:

```py
class A: ...

def f(x: type[A]):
    y = x()
    reveal_type(y)
```

But it's pretty easy to demonstrate that this is unsound:
- `type[]` is covariant, meaning that a subclass of `A` could be passed into `f` and red-knot would not complain
- subclasses of `A` are allowed to override `__init__` and/or `__new__` any way they like, since constructors are not checked for Liskov compatibility by type checkers.

```py
class B(A):
    def __init__(self, required_argument: int): pass

f(B)  # boom, but not detected by red-knot
```

The ability to call `type[]` types is useful for a variety of reasons, so we should allow it by default. However, we should add an opt-in diagnostic that warns users when they call `type[]` types, because of this unsoundness. When the rule is enabled, the behaviour would be something like this:


```py
class A: ...

def f(x: type[A]):
    y = x()  # error: [unsound-type-call] "Calling a `type[]` type is unsound as constructor methods are not checked for Liskov compatibility by type checkers"
    reveal_type(y)  # revealed: A
```

Originally suggested by @carljm in https://github.com/astral-sh/ruff/issues/15948. I think @mishamsk may be interested in working on this issue!

---

_Comment by @mishamsk on 2025-03-31 18:11_

@AlexWaygood thanks for creating a separate issue. Happy to help, so feel free to assign to me. But, I need to conquer my current nemesis astral-sh/ty#186 first

---

_Renamed from "[red-knot] Add an opt-in diagnostic warning about unsound calls to `type[]`" to "Add an opt-in diagnostic warning about unsound calls to `type[]`" by @MichaReiser on 2025-05-07 15:25_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:57_

---
