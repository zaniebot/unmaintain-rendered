```yaml
number: 2185
title: TypeIs does not narrow
type: issue
state: closed
author: RyanSaxe
labels:
  - narrowing
assignees: []
created_at: 2025-12-23T14:33:44Z
updated_at: 2025-12-23T15:51:28Z
url: https://github.com/astral-sh/ty/issues/2185
synced_at: 2026-01-10T01:56:41Z
```

# TypeIs does not narrow

---

_Issue opened by @RyanSaxe on 2025-12-23 14:33_

### Summary

Take the following simple program

```python
import random
from typing import TypeIs, reveal_type


def narrower(arg: int | str) -> TypeIs[int]:
    return isinstance(arg, int)


def function() -> int | str:
    return random.choice([42, "hello"])


variable = function()

if narrower(variable):
    reveal_type(variable)
```

By definition of `TypeIs`, if `narrower` returns `True`, then the type should be an `int`, but ty says `int | str`.

Please find a screenshot below for my editor using ty as well as basedpyright. Interestingly, ty has an additional diagnostic about `TypeIs` not being a member of `typing`, which may be what the bug is? Note that my python version is 3.13.1 and I am on the latest version of ty (0.0.5).

<img width="1028" height="424" alt="Image" src="https://github.com/user-attachments/assets/0dbe3f34-90ed-4bf4-b948-e6bc10b85a2e" />
<img width="885" height="424" alt="Image" src="https://github.com/user-attachments/assets/dcf7e04b-9465-422d-b659-e1243d95d9bc" />

### Version

ruff 0.14.10

---

_Label `narrowing` added by @AlexWaygood on 2025-12-23 14:49_

---

_Comment by @sinon on 2025-12-23 15:09_

`TypeIs` is supported by `ty` see: https://play.ty.dev/41cf2837-9688-46f4-81f1-315a22ae9415 if `TypeIs` is available in your python version. 

The behaviour of `ty` is that if you specify a supported python range in `pyproject.toml` e.g. ">= 3.11" it will typecheck at the lower bound (3.11 in this case) and not the python version installed in your virtual env. So if you go to the play link above and then adjust the python version to 3.11/3.12 you will then start seeing `str | int` and the `unresolved-import` that you are experiencing in your editor.

So the way to resolve is to include a python version check and then import `TypeIs` from `typing_extensions` when <= 3.13

Relevant doc: https://docs.astral.sh/ty/python-version/

---

_Comment by @RyanSaxe on 2025-12-23 15:27_

> `TypeIs` is supported by `ty` see: https://play.ty.dev/41cf2837-9688-46f4-81f1-315a22ae9415 if `TypeIs` is available in your python version.
> 
> The behaviour of `ty` is that if you specify a supported python range in `pyproject.toml` e.g. ">= 3.11" it will typecheck at 3.11 and not the python version installed in your virtual env. So if you go to the play link above and then adjust the python version to 3.11/3.12 you will then start seeing `str | int` and the `unresolved-import` that you are experiencing in your editor.
> 
> So the way to resolve is to include a python version check and then import `TypeIs` from `typing_extensions` when <= 3.13

Got it. This does resolve my simple example. However, it does not solve the problem for the actual use case I had. So let me share that instead of closing this Issue.

```python
from typing import ClassVar, Protocol, TypeIs, reveal_type
from typing import is_typeddict as _is_typeddict


class TypedDictProtocol(Protocol):
    __required_keys__: ClassVar[frozenset[str]]
    __optional_keys__: ClassVar[frozenset[str]]
    __readonly_keys__: ClassVar[frozenset[str]]
    __mutable_keys__: ClassVar[frozenset[str]]
    __orig_bases__: ClassVar[tuple[type, ...]]

    def __getitem__(self, key: str, /) -> object: ...
    def __contains__(self, key: object, /) -> bool: ...
    def __len__(self) -> int: ...

    # you get the point ... I wont show all the methods here

# the typing module does not provide narrowing, but since I have a library that regularly interacts with
# TypedDict internals, I have a Protocol that I use to represent one.
def is_typeddict(tp: type) -> TypeIs[type[TypedDictProtocol]]:
    return _is_typeddict(tp)


def do_something(tp: type):
    if is_typeddict(tp):
        reveal_type(tp)
```

other type checkers here properly get that the type of `tp` is `type[TypedDictProtocol]`, however `ty` just says it's of type `type`.

NOTE: from messing around it seems that this doesnt work in ty specifically for `TypeIs[type[SomeProtocol]]`, or something like that. Which I believe is fully supported from the PEP.

---

_Comment by @carljm on 2025-12-23 15:51_

Thanks for the report! We don't support `type[AProtocol]` yet (in general, nothing to do with `TypeIs` specifically). See #903. We should support it soon, it's just somewhat under-specified and there are some questions to sort out about how it should work.

---

_Closed by @carljm on 2025-12-23 15:51_

---
