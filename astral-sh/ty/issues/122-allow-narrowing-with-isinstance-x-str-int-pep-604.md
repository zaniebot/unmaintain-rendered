```yaml
number: 122
title: "Allow narrowing with `isinstance(x, str | int)` (PEP 604 union types)"
type: issue
state: closed
author: MatthewMckee4
labels:
  - narrowing
assignees: []
created_at: 2025-04-19T22:14:53Z
updated_at: 2025-11-08T18:20:48Z
url: https://github.com/astral-sh/ty/issues/122
synced_at: 2026-01-12T15:54:22Z
```

# Allow narrowing with `isinstance(x, str | int)` (PEP 604 union types)

---

_@MatthewMckee4_

Related:

What should be the expected revealed type here?
```py
from typing_extensions import reveal_type

class A: ...
class B: ...
  
reveal_type(A | B) # revealed: `UnionType` or `type[A] | type[B]`?
```

Either way i think we must store the value of `A | B` as a `Type::Union`.

The way `mypy` handles this is it uses a `uses_pep604_syntax` flag in the `UnionType` class which allows it to reveal `UnionType`.

So, im not sure what the functionality should be

---

_Comment by @AlexWaygood on 2025-04-20 10:43_

What do you mean in your issue title when you say "Allow `isinstance(x, str | int)`"? We already allow these calls as long as you have `--python-version=3.10` (or higher) set: https://playknot.ruff.rs/fe84b883-b0cd-42bf-93c4-4e4d8f42f710

It's true that `reveal_type(int | str)` reveals `UnionType` -- but this is accurate regarding what the type is at runtime:

```pycon
>>> type(int | str)
<class 'types.UnionType'>
```

Something we don't yet do is support narrowing inside these branches, e.g.

```py
def f(x: object):
    if isinstance(x, (int, str)):
        reveal_type(x)  # revealed: int | str

    if isinstance(x, int | str):
        reveal_type(x)  # revealed: object
```

https://playknot.ruff.rs/1f038049-dcbf-41ce-a0f4-e6ee8b15c778

It would obviously be great if the second `isinstance()` call narrowed the type in the same way as the first one.

---

_Renamed from "[red-knot] Allow `isinstance(x, str | int)` (PEP 604 union types)" to "[red-knot] Allow narrowing with `isinstance(x, str | int)` (PEP 604 union types)" by @MatthewMckee4 on 2025-04-20 10:45_

---

_Comment by @MatthewMckee4 on 2025-04-20 10:48_

Yeah, i meant regarding the narrowing.

---

_Comment by @MatthewMckee4 on 2025-04-20 10:50_

> Either way i think we must store the value of A | B as a Type::Union.
The way mypy handles this is it uses a uses_pep604_syntax flag in the UnionType class which allows it to reveal UnionType.
So, im not sure what the functionality should be

Do you have any thoughts on this?

---

_Comment by @AlexWaygood on 2025-04-20 14:31_

This is quite a hard problem. (That doesn't mean that you _shouldn't_ work on it -- though you of course don't have to -- it's just that there's a number of things to think through!)

I don't think we should infer the type of `A | B` here as a `Type::Union` because in contexts where we need to treat the object as a value expression that might lead to incorrect inferences. For example:

```py
class A:
    x = 42

class B:
    x = 42

C = A | B

reveal_type(C.x)  # if we inferred `C` as having type `A | B`,
                  # we would fail to emit an error here and would reveal `Literal[42]`.
                  # But it fails at runtime! Because at runtime, `C` has type `types.UnionType`.
```

One solution here might be a new `Type` variant to represent these `types.UnionType` instances? We need to continue treating them as "just instances of `types.UnionType`" in the vast majority of cases, but in specific situations (like `isinstance()` narrowing), we need to be able to know what the original classes where that were `|`'d together to create the `UnionType` instance.

We have a similar problem for lots of other special typing constructs. For example:

```py
from typing import Literal

X = Literal["foo", "bar"]

def f(x: X):
    reveal_type(x)
```

At runtime, the type of `X` is `typing._LiteralGenericAlias`. That's not a class that even exists in typeshed; typeshed just says that `typing.Literal` is an instance of `_SpecialForm`, and it just says that all instances of `_SpecialForm` return `object` from their `__getitem__` method. In most cases, we want to treat `Literal["foo", "bar"]` as an opaque object (which is why typeshed tells us to infer `object` for it), but in specific cases (where it's used in a type expression), we want to get back the original strings that were used to subscript `Literal`.

So I could see there being a more general solution there that doesn't just fix things for `UnionType`s: a `Type` variant that keeps track of the runtime type of the object as well as the type we should use whenever the object is used in a type expression:

```rs
struct TypeForm<'db> {
    runtime_type: Type<'db>,
    in_type_expression: Type<'db>
}
```

where the union `A | B` would be represented as 

```rs
TypeForm {
    runtime_type: KnownClass::UnionType.to_instance(db),
    in_type_expression: UnionType::from_elements(db, [A, B]),
}
```

cc. @dcreager, who I know has been doing a fair bit of thinking about how to represent typeforms in red-knot's model recently

---

_Comment by @MatthewMckee4 on 2025-04-20 19:44_

Ah okay this makes a lot of sense, but yeah i see why its quite a hard issue that can be generalised to fix more issues, thanks for responding, it seems like i should leave this to the pros!

---

_Renamed from "[red-knot] Allow narrowing with `isinstance(x, str | int)` (PEP 604 union types)" to "Allow narrowing with `isinstance(x, str | int)` (PEP 604 union types)" by @MichaReiser on 2025-05-07 15:25_

---

_Label `narrowing` added by @AlexWaygood on 2025-05-10 18:07_

---

_Closed by @AlexWaygood on 2025-11-08 18:20_

---
