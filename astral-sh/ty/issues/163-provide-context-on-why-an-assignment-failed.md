```yaml
number: 163
title: Provide context on why an assignment failed
type: issue
state: open
author: dhruvmanila
labels:
  - diagnostics
assignees: []
created_at: 2025-03-22T21:08:14Z
updated_at: 2026-01-11T16:36:57Z
url: https://github.com/astral-sh/ty/issues/163
synced_at: 2026-01-12T15:54:22Z
```

# Provide context on why an assignment failed

---

_@dhruvmanila_

It will be useful to provide why an assignment between two types failed. Pyright does this but for ty it will require updating the APIs because currently the returned type is boolean for `is_assignable_to`.

---

_Label `diagnostics` added by @dhruvmanila on 2025-03-22 21:08_

---

_Renamed from "[red-knot] Provide context on why an assignment failed" to "Provide context on why an assignment failed" by @MichaReiser on 2025-05-07 15:26_

---

_Comment by @MatthewMckee4 on 2025-07-27 10:28_

Would we be looking to create an enum to represent a reason for a lack of assignability/subtyping?

Then return 

```rust
enum RelationResult {
    Pass
    Fail(Reason)
}

```

---

_Comment by @AlexWaygood on 2025-07-27 11:06_

Possibly `has_relation_to` could return `Result<(), AssignabilityError>`:
- If the first type is a subtype of (or assignable to) the second type, `Ok(())` is returned
- If the first type doesn't have the given relation to the second type, however, `Err(Assignability)` is returned, where `AssignabilityError` is an enum that provides context on why the relation doesn't exist

---

_Comment by @MatthewMckee4 on 2025-07-27 14:08_

Sounds good.

Just wondering but what is the benefit of using a result for this. I think I'm right in saying `Result<(), ...>` is *equivalent* to `Option<...>` and also to my example. What makes you choose any one of these?

---

_Comment by @MatthewMckee4 on 2025-07-27 14:11_

Also do we not want the error type to represent subtyping issues too?

---

_Comment by @AlexWaygood on 2025-07-27 14:17_

> Just wondering but what is the benefit of using a result for this. I think I'm right in saying `Result<(), ...>` is _equivalent_ to `Option<...>` and also to my example. What makes you choose any one of these?

I don't think using an `Option` or a "top-level" enum would be _wrong_ -- but if an operation could succeed or fail (and we want to provide context for why it failed in the failure case), it seems natural to me to use a `Result` to represent that. It's the type that is generally used for this kind of situation, and it has language support built around it (e.g. you can use the `?` operator to propagate errors upwards, and `map_err` to convert one kind of error into another).

> Also do we not want the error type to represent subtyping issues too?

Yes, good point! `TypeRelationError` might be a better name.

---

_Comment by @MatthewMckee4 on 2025-07-27 14:25_

Okay sounds good, I'd be happy to set this up, it seems like we need a sort of base case for the enum (or a todo variant, or both?) to start.

---

_Comment by @MatthewMckee4 on 2025-07-27 21:16_

Making a small start on this. It seems fair to be able to allow multiple reasons, for union types for example.

What do we think of this as a base for `TypeRelationError`?

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub(crate) struct TypeRelationError(pub(crate) Vec<TypeRelationErrorKind>);

impl TypeRelationError {
    pub(crate) fn single(kind: TypeRelationErrorKind) -> Self {
        Self(vec![kind])
    }
}

#[derive(Debug, Copy, Clone, PartialEq, Eq)]
pub(crate) enum TypeRelationErrorKind {
    Todo,
    GradualTypeInSubTyping,
}

```

---

_Comment by @AlexWaygood on 2025-07-27 21:18_

That looks like a reasonable design to me!

---

_Comment by @MatthewMckee4 on 2025-07-27 21:58_

Should this also cover is_equivalent_to functions?

---

_Comment by @AlexWaygood on 2025-07-27 22:08_

Maybe as a followup, but I think this task in itself will involve some refactoring, so I wouldn't expand the scope for the initial PR

---

_Comment by @jelle-openai on 2025-07-28 14:40_

> Possibly `has_relation_to` could return `Result<(), AssignabilityError>`:
> 
> * If the first type is a subtype of (or assignable to) the second type, `Ok(())` is returned
> * If the first type doesn't have the given relation to the second type, however, `Err(Assignability)` is returned, where `AssignabilityError` is an enum that provides context on why the relation doesn't exist

For what it's worth, this is quite similar to what I do in pycroscope, except that the error case is nested ([code here](https://github.com/JelleZijlstra/pycroscope/blob/0d19236e4eda771175170a6b165b0e9f6a211d19/pycroscope/value.py#L342)).

The relevant function returns either:
- A dictionary providing bounds on TypeVars that need to apply for the assignment to work
- Or a CanAssignError object, which contains a string plus a list of deeper CanAssignError objects

The latter gets displayed with nested indented lists:

```
In [8]: typ = pycroscope.annotations.type_from_runtime(tuple[str, int] | tuple[str, float])

In [10]: typ.can_assign(pycroscope.value.KnownValue((1, "x")), pycroscope.runtime._get_checker())
Out[10]: CanAssignError(message="Literal[(1, 'x')] is not assignable to tuple[str, int] | tuple[str, float]", children=[CanAssignError(message='Elements at position 0 are not compatible', children=[CanAssignError(message='Cannot assign Literal[1] to str', children=[], error_code=None)], error_code=None), CanAssignError(message='Elements at position 0 are not compatible', children=[CanAssignError(message='Cannot assign Literal[1] to str', children=[], error_code=None)], error_code=None)], error_code=None)

In [11]: print(_)
  Literal[(1, 'x')] is not assignable to tuple[str, int] | tuple[str, float]
    Elements at position 0 are not compatible
      Cannot assign Literal[1] to str
    Elements at position 0 are not compatible
      Cannot assign Literal[1] to str
```
(Maybe not a great example because it doesn't tell you which arm of the union corresponds to the indented parts. I'm sure you can do better there :). But I think the idea of building up a tree of reasons for why the assignment didn't work is useful.)

---

_Comment by @AlexWaygood on 2025-07-29 11:00_

If we consider the example I gave in https://github.com/astral-sh/ty/issues/866:

```py
from typing import Callable, Any

def f(x: Callable[[Any], Any]): ...

class Foo:
    def __init__(self, x, y): ...

f(Foo)
```

Mypy's diagnostic also isn't _great_ in this case:

> ```
> error: Argument 1 to "f" has incompatible type "type[Foo]"; expected "Callable[[Any], Any]"  [arg-type]
> ```

https://mypy-play.net/?mypy=latest&python=3.12&gist=ee7c1164dc400825966268856211fa15

But pyright's is much better here ('Extra parameter "y"'):

> ```
> Argument of type "type[Foo]" cannot be assigned to parameter "x" of type "(Any) -> Any" in function "f"
>   Type "type[Foo]" is not assignable to type "(Any) -> Any"
>     Extra parameter "y"  (reportArgumentType)
> ```

https://pyright-play.net/?pythonVersion=3.13&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCAhgDYlEBGJApgDRQCCKcAUCwCbXBTAAUAHgC5CpclWoBtCUzgBdejNkBKYQDp1bAMbkAzjqgAxMGEEso5qJ24B9a6iQxbvHdRLB6-enBVR1qtnxGYEosQA

For a similar case involving `Protocol`s, meanwhile, mypy's diagnostic is superior IMO:

```py
from typing import Protocol, Any

class Bar(Protocol):
    def __call__(self, x: Any) -> Any: ...
    y: int

def f(x: Bar): ...

class Foo:
    def __call__(self, x: Any, y: Any) -> Any: ...
    y: int

f(Foo())
```

Mypy says:

```
main.py:13: error: Argument 1 to "f" has incompatible type "Foo"; expected "Bar"  [arg-type]
main.py:13: note: Following member(s) of "Foo" have conflicts:
main.py:13: note:     Expected:
main.py:13: note:         def __call__(self, x: Any) -> Any
main.py:13: note:     Got:
main.py:13: note:         def __call__(self, x: Any, y: Any) -> Any
main.py:13: note: "Bar.__call__" has type "Callable[[Arg(Any, 'x')], Any]"
```

Pyright says:

```
Argument of type "Foo" cannot be assigned to parameter "x" of type "Bar" in function "f"
  "Foo" is incompatible with protocol "Bar"
    "__call__" is an incompatible type
      Type "(x: Any, y: Any) -> Any" is not assignable to type "(x: Any) -> Any"
        Extra parameter "y"  (reportArgumentType)
```

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:28_

---

_Comment by @carljm on 2025-11-19 20:20_

https://github.com/astral-sh/ty/issues/1591 is another example case to consider here, where pyright provides a fantastic diagnostic.

[Pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0AFag1wBjXFAA0dACrMYANVSVpuAG4xKUXKkzSAyjACOAVxjpRMaQEkGm1CJV0AgulIGTxHnzjWsMfAAddGCAfRlQ8ToAXll5JUoACkCQcMjcFOlxNWUIDAZomUozAEpg8osoVDg4OkNTc0sAOVwGfQYkoVaxCQBtNPEAXRLEYLpxugABdU1tXTGJzBgwOlDQthgGCDsaNcS4GCgwaVYl-EQ6z28GX398aQB6EroAWgA%2BVYjxC8JfiompjMtDpMAtxksVmsNlsdnsDkcTncLnAoBBLI9nu86sYzBYYP0vrhBj8-iF0ACIat0ugGKhWHA4YdjnQclAzBdcNgAFYwUQMDGvD54CQkwj-RbLKmwdCMo6Yj6sBii8XgyVrbaaWVgeV0Wz2RwE9LEui-MVkimS04BfZM6Ss9kuNyPaRwWl8C6KmJ0AAMLpExA9NK9pp1iuV5olK3EJhpNoRLNQbJgF1c7joT0FnBp4bBdEpa0oMA0lAOmC1Or1lAcvENQxz-hWYES52xDTxLTaHV6rsow3rTd6KVQmToKWwI5SohSwwq%2BebF1Rrt6qboAB86D2%2B7mm-gyugQJIQCYtlA4CRyIgQABiOgAVRP21IdDAMb5EFw6Dgs8lYF4NAcoToCYNDYJo85ZgwOo9qM5ITIWDAmJQ5JgCkTTAaBlAXMA%2BAAL4pMEB4gGQhZgFApCECINBQBQN4CKQJFkRuGA4AQdDiOgkBsIhDjvugZo3oYMB0AAFgwDDEHAiAPA8xHLGRhC8GwDzmA8mBiHADzsZx3FbB%2BDzPrwdCoDk0CoNgsBsR%2B2lVrp5K4MQtlnsEZAMMJH4vMWcC8V6KQAMyEAAjAATPh%2B44YeqBvhoABi0AwBQaBYHgRBkCAOFAA) also does alright here -- it doesn't provide the full chain like pyright does, but it gives you the inner-most detail.

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:38_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:38_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-11 16:36_

---
