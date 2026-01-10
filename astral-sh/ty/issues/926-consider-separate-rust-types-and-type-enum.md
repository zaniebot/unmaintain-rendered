```yaml
number: 926
title: "Consider separate Rust types and `Type` enum variants for bound vs unbound type variables"
type: issue
state: closed
author: dcreager
labels:
  - generics
assignees: []
created_at: 2025-08-01T15:01:47Z
updated_at: 2025-08-11T19:29:59Z
url: https://github.com/astral-sh/ty/issues/926
synced_at: 2026-01-10T02:06:24Z
```

# Consider separate Rust types and `Type` enum variants for bound vs unbound type variables

---

_Issue opened by @dcreager on 2025-08-01 15:01_

"Bind" can confusingly mean ~3 different things when talking about Python type variables. Here I mean that a legacy typevar is _defined_ before it is _used_ (or "bound"), and that it can be used/bound multiple times:

```py
from typing import Generic, TypeVar

T = TypeVar("T")                                   # [1]

def first_binding(t: T) -> T: ...                  # [2]

class SecondBinding(Generic[T]):                   # [3]
    def not_a_new_binding(self, t: T) -> T: ...
```

As of https://github.com/astral-sh/ruff/pull/19604, we create a `TypeVarInstance` for the legacy typevar when it is first defined (`[1]`), and then additional distinct `TypeVarInstance`s each time it is used/bound (`[2]` and `[3]`).

@MichaReiser suggested trying to use separate Rust types to track the unbound and bound typevars. This turned out to be a beefy enough change that we decided not to fold it into https://github.com/astral-sh/ruff/pull/19604, but it is still worth considering as follow-on work. The hope is that this would bring a performance win, since `TypeVarInstance` is a salsa-interned struct that we are now creating many more instances of. (We would presumably _not_ have salsa track or intern the proposed `BoundTypeVarInstance`.)

Note that it would require separate `Type` variants for bound and unbound typevars, since there are places where we need to refer to (the type of) the legacy typevar before it has been bound:

```py
from typing import Generic, TypeVar

T = TypeVar("T")
U = TypeVar("U", default=T)
```

---

_Label `generics` added by @dcreager on 2025-08-01 15:01_

---

_Comment by @carljm on 2025-08-06 21:03_

> Note that it would require separate `Type` variants for bound and unbound typevars

I'm a little confused about this part. Do we need any more `Type` variants than we already have?

We already have `Type::TypeVar` for "the unknown (possibly bounded/constrained) type spelled by a use of a typevar as a type annotation in a scope where it is bound" -- this should _always_ contain a bound typevar. And we also already have `Type::KnownInstance(KnownInstanceType::TypeVar(...))` for "the actual `typing.TypeVar` instance itself, as a runtime Python object". I think this latter should always contain an unbound typevar, though it's less clear whether it matters, since "boundness" is meaningless in this case. Consider:

```py
T = TypeVar("T")

def f(x: T) -> T:
    y = T
    z: T = x
    return z
```

the type of `y` should be `Type::KnownInstance(KnownInstanceType::TypeVar(<T>))`, it should not be `Type::TypeVar(<T>)` (because this is a value-expression load of the object referenced by the name T, which is simply a regular instance of the class `typing.TypeVar`; `y` does not have a generic type). I think ideally the TypeVarInstance wrapped here would not be bound, because boundness has no meaning here. In `z: T = x`, however, `z` should have a `Type::TypeVar` type, which should definitely wrap a bound TypeVarInstance.

I feel like perhaps this calls into question the strategy of binding typevars in `infer_place_load` in the first place; it seems like maybe we should really only ever be binding them in `in_type_expression`?

---

_Comment by @dcreager on 2025-08-07 01:04_

I've replied to the above comments in https://github.com/astral-sh/ruff/pull/19796, which implements this.

---

_Closed by @dcreager on 2025-08-11 19:30_

---
