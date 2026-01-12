```yaml
number: 1747
title: "Consider disallowing forward references to non-global names in `__future__.annotations` (for compatibility)"
type: issue
state: open
author: BHSPitMonkey
labels:
  - diagnostics
  - needs-decision
assignees: []
created_at: 2025-12-03T19:58:15Z
updated_at: 2025-12-22T17:59:59Z
url: https://github.com/astral-sh/ty/issues/1747
synced_at: 2026-01-12T15:54:25Z
```

# Consider disallowing forward references to non-global names in `__future__.annotations` (for compatibility)

---

_@BHSPitMonkey_

### Summary

Examples: https://play.ty.dev/e6628610-7be6-4090-82ca-d1cadf92e85f

Beginning with the `alpha.28` release (and still happening as of `alpha.30`), ty emits `invalid-type-form` for valid types that happen to share a name with an instance method.

Example involving the `set` builtin:

```python
class Foo:
  def set(self) -> None:
    pass

  def bar(self) -> set[int]:  # invalid-type-form
    return {1, 2, 3}

  def baz(self) -> set:       # invalid-type-form
    return {"hello", "world"}
```

```
Invalid subscript of object of type `def set(self) -> None` in type expression
Variable of type `def set(self) -> None` is not allowed in a type expression
```

Example using a non-builtin type:

```python
from datetime import datetime

class Foo:
  def datetime(self) -> None:
    pass

  def maybe_return_date(self) -> datetime | None:  # invalid-type-form
    return None
```

```
Variable of type `def datetime(self) -> None` is not allowed
```

### Version

ty 0.0.1-alpha.28, ty 0.0.1-alpha.29, ty 0.0.1-alpha.30

---

_Label `question` added by @sharkdp on 2025-12-03 20:02_

---

_Comment by @sharkdp on 2025-12-03 20:04_

Thank you for reporting this.

Pyright seems to support this, but mypy and pyrefly also think that something is wrong here. I think it's reasonable for `ty` to assume that `set` refers to the *symbol in the same scope of the same name*, even if that happens to be a function definition. I would advise to disambiguate the name of the type by using an `as`-import, a type alias, a fully qualified name or similar.

---

_Comment by @carljm on 2025-12-03 20:20_

Yes, this is an intentional choice in ty to respect normal Python name-resolution rules for names in type annotations, rather than special-casing some logic to skip the local scope and prefer the global scope. The ty behavior also matches the runtime behavior of resolving `__annotations__`:

```
>>> class Foo:
...   def set(self) -> None:
...     pass
...
...   def bar(self) -> set[int]:  # invalid-type-form
...     return {1, 2, 3}
...
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    class Foo:
    ...<4 lines>...
        return {1, 2, 3}
  File "<python-input-0>", line 5, in Foo
    def bar(self) -> set[int]:  # invalid-type-form
                     ~~~^^^^^
TypeError: 'function' object is not subscriptable
```

IMO it's unfortunate that there has been confusion in the past around these name resolution rules (contributed to both by type-checkers and the behavior of `typing.get_type_hints` with string annotations), but I think it's clear that using normal resolution rules is the least confusing and most consistent path forward.

---

_Closed by @carljm on 2025-12-03 20:21_

---

_Comment by @BHSPitMonkey on 2025-12-03 20:43_

Thanks for clarifying, that makes sense! I only thought to compare against Pyright's behavior, but I see what you mean about adhering to how Python treats the (class) local scope here. I'll get around this by using `-> builtins.set[...]` in our code.

---

_Comment by @correctmost on 2025-12-03 21:05_

I have seen a similar `invalid-type-form` warning in the archinstall codebase where ty is the only type checker to issue a warning:

```python
from __future__ import annotations
import uuid
from enum import Enum


class PartitionGUID(Enum):
    @property
    def bytes(self) -> bytes:  # error[invalid-type-form]: Variable of type `property` is not allowed in a type expression
        return uuid.UUID(self.value).bytes
```

Pyrefly, Pyright, mypy, and zuban do not emit type-form warnings for the same code, and the code also works at runtime.  (ty does not warn about the code if `from __future__ import annotations` is removed.)

I don't know if that possibly changes this bug's resolution, but I thought I'd at least mention a case where ty seems to differ in behavior from other major type checkers.

---

_Comment by @BHSPitMonkey on 2025-12-03 21:17_

@correctmost That's the same case discussed above; It works at runtime because Python doesn't enforce type annotations, but in the `class PartitionGUID` scope the Python runtime would interpret `bytes` as a reference to the property instead of the builtin (e.g. if you added a class variable `foo = bytes`, it would refer to the property).

You can use the same workaround I'm using, e.g.:

```python
from __future__ import annotations
import builtins
import uuid
from enum import Enum


class PartitionGUID(Enum):
    @property
    def bytes(self) -> builtins.bytes:  # No error
        return uuid.UUID(self.value).bytes
```

---

_Comment by @carljm on 2025-12-03 22:06_

Thanks @correctmost for the additional example!

For stringified annotations (`from __future__ import annotations`), runtime never even evaluates the annotation. `typing.get_type_hints` will evaluate the stringified annotation, but it has a special-case internally to swap the local and global namespaces, so the global namespace is preferred over the local one.

This example works for mypy, pyrefly, and zuban for an entirely different reason than it "works" in `get_type_hints`. They do not implement the same namespace-swapping that `get_type_hints` does (and pyright does), which is clear because they all complain about the annotation on `foo` here:

```py
from __future__ import annotations

class C:
    def bytes(self) -> bytes:  # mypy, pyrefly, zuban are all OK with this
        return b""

    def foo(self) -> bytes:  # mypy, pyrefly, zuban all error here
        return self.bytes()
```

At runtime, names in type annotations with `from __future__ import annotations` are only evaluated if you explicitly ask for their evaluation, and not immediately. This means they can be forward references. Ty models this accurately, and thus (with `from __future__ import annotations`) we consider the annotation `bytes` in `def bytes(self) -> bytes:` to be a forward-reference to that same method.

Note that in Python 3.14, with PEP 649/749, annotations are deferred by default (without stringifying them), and `from __future__ import annotations` will become deprecated in future. And the behavior of Python 3.14 at runtime matches ty's behavior:

```
Python 3.14.0rc2 (main, Aug 28 2025, 17:02:21) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> class C:
...     def bytes(self) -> bytes:
...         return b""
...
>>> C.bytes.__annotations__
{'return': <function C.bytes at 0x1048607d0>}
```

So it still seems to me that ty's behavior here is the most consistent, and that the best thing to do with such annotations is to disambiguate them, which will give full compatibility across all type checkers and future runtime behavior.

---

_Comment by @carljm on 2025-12-03 22:14_

Since this is clearly a subtle issue that other people are likely to run into, and there's at least some possibility that we might decide to make our behavior less internally consistent in order to improve compatibility with other type checkers (though at the moment I'm not convinced we should do this), I'll go ahead and reopen this for visibility.

---

_Reopened by @carljm on 2025-12-03 22:14_

---

_Label `question` removed by @carljm on 2025-12-03 22:14_

---

_Label `needs-decision` added by @carljm on 2025-12-03 22:14_

---

_Renamed from "Spurious `invalid-type-form` when instance method shares name with a type" to "Consider disallowing forward references to non-global names in `__future__.annotations` (for compatibility" by @carljm on 2025-12-03 22:17_

---

_Comment by @carljm on 2025-12-03 22:19_

To better illustrate the difference between ty and other type checkers, ty supports this forward reference (in Python 3.14, or if `from __future__ import annotations` is active):

```py
class C:
    def foo(self) -> IntAlias:
        return 1

    IntAlias = int
```

Whereas mypy/pyrefly/zuban all error that `IntAlias` is not defined (but are fine if `IntAlias` is defined above `foo` instead). This is what leads to the difference in behavior on @correctmost 's example: ty is the only one to support forward references in non-global scope.

But at runtime in Python 3.14, the above works fine and the return annotation in `__annotations__` resolves to `int`.

---

_Comment by @MichaReiser on 2025-12-03 22:52_

It might be worth improving our diagnostics if we dedect this very specific issue 

---

_Comment by @correctmost on 2025-12-03 23:07_

> This example works for mypy, pyrefly, and zuban for an entirely different reason than it "works" in `get_type_hints`. They do not implement the same namespace-swapping that `get_type_hints` does (and pyright does)

Thank you for the detailed explanation about the various subtleties :).

---

_Label `diagnostics` added by @MichaReiser on 2025-12-04 06:15_

---

_Renamed from "Consider disallowing forward references to non-global names in `__future__.annotations` (for compatibility" to "Consider disallowing forward references to non-global names in `__future__.annotations` (for compatibility)" by @carljm on 2025-12-18 00:19_

---

_Comment by @konn on 2025-12-19 08:43_

Hi, I'm just redirected from #2101. We have a
case where all the classes/functions are defined in the same module, and that stub file has thousands of auto-generated lines.

Much simplified version is as follows:

```py
class A: ...

class B:
    def A(self, name: str) -> A: ...
    def take_a(self, a: A) -> str: ...
    def return_a(self) -> A: ...
```

In this monolithic module case, I think there is no way to refer class `A` defined toplevel inside class B, IIUC. Since these codes are machine-generated, it is rather hard to reorder declarations. I think it does still make sense to provide some workaround for this case. 

---

_Comment by @carljm on 2025-12-22 17:59_

@konn In your original example, every type checker except pyright will error on the return annotation of `return_a` method, see e.g. mypy: https://mypy-play.net/?mypy=latest&python=3.12&gist=032b3775e8d655314c535884b76b36e2

Pyright doesn't error because it copies the local/global namespace swap performed by `typing.get_type_hints`. It does this even when `from __future__ import annotations` is not active, which means it gives results which don't match the runtime resolution of annotations.

The workaround available in your case (or any similar case) is to alias the name to remove ambiguity:

```py
class A: ...
_AliasToA = A

class B:
    def A(self, name: str) -> _AliasToA: ...
    def take_a(self, a: A) -> str: ...
    def return_a(self) -> _AliasToA: ...
```



---
