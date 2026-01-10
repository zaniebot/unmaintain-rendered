```yaml
number: 116
title: "Subscripted generic classes should not be considered instances of `type`"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - generics
  - type properties
assignees: []
created_at: 2025-04-22T13:45:24Z
updated_at: 2025-12-18T20:34:09Z
url: https://github.com/astral-sh/ty/issues/116
synced_at: 2026-01-10T01:53:59Z
```

# Subscripted generic classes should not be considered instances of `type`

---

_Issue opened by @AlexWaygood on 2025-04-22 13:45_

### Summary

ty currently emits no error on the following code, but it should:

```py
class Foo[T]: ...

def requires_type(x: type): ...

requires_type(Foo[int])
```

`Foo[int]` here is not an instance of `type`. In the long run, it'll be quite important for us to understand this, as the runtime frequently distinguishes between generic aliases and instances of types:

<details>
<summary>REPL session demonstrating several places where generic aliases are not accepted as instances of `type`</summary>

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class Foo[T]: ...
... 
>>> isinstance(object(), Foo[int])
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    isinstance(object(), Foo[int])
    ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1375, in __instancecheck__
    return self.__subclasscheck__(type(obj))
           ~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1378, in __subclasscheck__
    raise TypeError("Subscripted generics cannot be used with"
                    " class and instance checks")
TypeError: Subscripted generics cannot be used with class and instance checks
>>> issubclass(object, Foo[int])
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    issubclass(object, Foo[int])
    ~~~~~~~~~~^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1378, in __subclasscheck__
    raise TypeError("Subscripted generics cannot be used with"
                    " class and instance checks")
TypeError: Subscripted generics cannot be used with class and instance checks
>>> from functools import singledispatch
>>> @singledispatch
... def f(arg): ...
... 
>>> @f.register(list[int])
... def f(arg): ...
... 
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    @f.register(list[int])
     ~~~~~~~~~~^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/functools.py", line 893, in register
    raise TypeError(
    ...<3 lines>...
    )
TypeError: Invalid first argument to `register()`: list[int]. Use either `@register(some_class)` or plain `@register` on an annotated function.
```

</details>

This bug can also be seen in that both these assertions currently pass, when they should fail:

```py
from ty_extensions import static_assert, is_subtype_of, is_assignable_to, TypeOf

class Foo[T]: ...

static_assert(is_subtype_of(TypeOf[Foo[int]], type))
static_assert(is_assignable_to(TypeOf[Foo[int]], type))
```
https://play.ty.dev/d373a056-b58c-451b-a136-323e76970454

Cc. @dcreager for generics!

---

_Label `bug` added by @AlexWaygood on 2025-04-22 13:45_

---

_Renamed from "[red-knot] Subscripted generic classes should not be considered instances of `type`" to "Subscripted generic classes should not be considered instances of `type`" by @MichaReiser on 2025-05-07 15:25_

---

_Label `generics` added by @AlexWaygood on 2025-05-10 18:05_

---

_Label `subtyping/assignability` added by @AlexWaygood on 2025-05-10 18:05_

---

_Comment by @Kroppeb on 2025-06-17 22:50_

Are you sure? Pyright and MyPy have the same behaviour.

---

_Comment by @jelle-openai on 2025-06-18 14:30_

Yes, Alex is correct here and mypy and pyright are wrong.

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 14:58_

---

_Added to milestone `GA` by @carljm on 2025-08-15 14:58_

---

_Comment by @carljm on 2025-12-18 20:34_

Adding a note here (for the benefit of issue search) that this is how we should also emit a diagnostic on e.g. `isinstance(x, list[str])`

---
