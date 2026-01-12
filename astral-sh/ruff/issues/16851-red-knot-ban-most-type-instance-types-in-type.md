```yaml
number: 16851
title: "[red-knot] Ban most `Type::Instance` types in type expressions"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-03-19T16:00:41Z
updated_at: 2025-03-20T22:19:58Z
url: https://github.com/astral-sh/ruff/issues/16851
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Ban most `Type::Instance` types in type expressions

---

_@AlexWaygood_

If a symbol that we infer as having an instance type appears in a type expression, we should almost always emit a diagnostic saying that it's an invalid type expression. For example:

```py
def f(x: int):
    def g(y: x): ...  # the annotation for `y` here is invalid:
                      # `x` has a `Type::Instance` type.
                      # We need to emit a diagnostic for type expressions like this
```

Making this change involves changing this branch of `Type::in_type_expression()` so that it returns an `Err()` in most cases:

https://github.com/astral-sh/ruff/blob/98fdc0ebae5b1f8279d2400445acaf3f172a6513/crates/red_knot_python_semantic/src/types.rs#L3328-L3330

But we need to make sure we avoid emitting an error if an instance of a `TypeVar`, `TypeVarTuple` or `ParamSpec` is used in a type expression, e.g.

```py
from typing import TypeVar

T = TypeVar("T")

def f(x: T) -> T: ...  # annotation for `x` is valid here,
                       # despite the fact that we currently infer it as a `Type::Instance` type
```

To avoid these false positives, we should probably do something like this for the branch:

```rs
            Type::Instance(InstanceType { class }) => match class.known(db) {
                Some(KnownClass::TypeVar) => Ok(todo_type!("Support for `typing.TypeVar` instances in type expressions")),
                Some(KnownClass::ParamSpec) => Ok(todo_type!("Support for `typing.ParamSpec` instances in type expressions")),
                Some(KnownClass::TypeVarTuple) => Ok(todo_type!("Support for `typing.TypeVarTuple` instances in type expressions")),
                _ => Err(...)
            }
```

Note that we do not yet have `ParamSpec` and `TypeVarTuple` variants in our `KnownClass` enum, but they probably need to be added in order to complete this task.

---

_Label `help wanted` added by @AlexWaygood on 2025-03-19 16:00_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-19 16:00_

---

_Closed by @carljm on 2025-03-20 22:19_

---

_Closed by @carljm on 2025-03-20 22:19_

---
