---
number: 13796
title: "[red-knot] support stringified annotations"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-10-17T16:41:08Z
updated_at: 2024-11-15T04:10:20Z
url: https://github.com/astral-sh/ruff/issues/13796
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] support stringified annotations

---

_Issue opened by @carljm on 2024-10-17 16:41_

As a work-around for forward references (given that by default type annotations are eagerly evaluated at runtime), the Python type system supports type annotations written as string literals, where the contents of the string should be interpreted by the type checker as an annotation expression. See https://typing.readthedocs.io/en/latest/spec/annotations.html#string-annotations

See https://github.com/astral-sh/ruff/discussions/13499 for prior discussion here.

Some salient things to consider:

## Which strings should be considered stringified annotations?

<details>

All string literals found within a type annotation (variable annotation or function parameter/return annotation) should be parsed as stringified annotations. (This is recursive, not only when the entire annotation is a string: parts of annotations can be stringified, e.g. `list["MyType"]`.)

EXCEPT that a string found inside `typing.Literal["some random string"]` or `typing.Annotated[SomeType, "some string"]` in an annotation should not be parsed as a stringified annotation. The difficulty here is that `foo["some random string"]` is also a string literal type, not a stringified annotation, if `foo` is actually an alias for `typing.Literal`, even via multiple levels of aliasing and imports. This means we don't know for sure if a string in an annotation is a stringified annotation until we are actually doing type inference on that code, so we can reliably identify `typing.Literal` and `typing.Annotated`.

</details>

## How should names within stringified annotations be resolved?

<details>

The typing spec says only:

> The local and global namespace in which it is evaluated should be the same namespaces in which default arguments to the same function would be evaluated.

In practice, this isn't how all type checkers behave, though. We can evaluate the actual behavior using this small module:

```py
from typing import TypeAlias, TYPE_CHECKING, get_type_hints

A: TypeAlias = int

class C:
    A: TypeAlias = str
    x: "A"
    y: A

    def f(self, x: "A", y: A):
        pass

if TYPE_CHECKING:
    reveal_type(C.x)
    reveal_type(C.y)
    reveal_type(C.f)
else:
    print(get_type_hints(C))
    print(get_type_hints(C.f))
```

We can evaluate this program against mypy, pyright, and the runtime behavior of `typing.get_type_hints` function. We find that only mypy follows the spec as written above, implementing name lookup for stringified annotations using the same scopes as for non-stringified annotations:

```
➜ mypy .
res.py:14: note: Revealed type is "builtins.str"
res.py:15: note: Revealed type is "builtins.str"
res.py:16: note: Revealed type is "def (self: res.C, x: builtins.str, y: builtins.str) -> Any"
```

However, `typing.get_type_hints` at runtime implements a backwards-compatibility hack to prioritize the global namespace over the local one, and pyright has [followed its lead](https://github.com/microsoft/pyright/discussions/8958):

```
➜ python3 res.py
{'A': typing.TypeAlias, 'x': <class 'int'>, 'y': <class 'str'>}
{'x': <class 'int'>, 'y': <class 'str'>}
```

```
➜ pyright
/Users/carlmeyer/projects/pyright-testing/res.py
  /Users/carlmeyer/projects/pyright-testing/res.py:14:17 - information: Type of "C.x" is "int"
  /Users/carlmeyer/projects/pyright-testing/res.py:15:17 - information: Type of "C.y" is "str"
  /Users/carlmeyer/projects/pyright-testing/res.py:16:17 - information: Type of "C.f" is "(self: C, x: int, y: str) -> None"
```

They both consider the type of `x` (either as a class attribute or a method parameter annotation) to be `int`, prioritizing the global namespace over the local one.

Pyright also does the same for non-stringified annotations which are deferred-evaluated because they are in a type stub, because the typing spec currently says that all annotations in a stub file should be treated as stringified:

```py
# file res1.py
from typing import TypeAlias

A: TypeAlias = int

class C:
    A: TypeAlias = str
    x: "A"
    y: A

reveal_type(C.x)
reveal_type(C.y)
```

```
➜ pyright
/Users/carlmeyer/projects/pyright-testing/res1.pyi
  /Users/carlmeyer/projects/pyright-testing/res1.pyi:10:13 - information: Type of "C.x" is "int"
  /Users/carlmeyer/projects/pyright-testing/res1.pyi:11:13 - information: Type of "C.y" is "int"
```

And pyright does the same for all annotations in a Python file when `from __future__ import annotations` is turned on, which makes sense, since at runtime those are actually all stringified annotations.

It's not totally clear to me what we should do here. I think I would _prefer_ to not implement this namespace swapping at all (like mypy), but maybe compatibility with `typing.get_type_hints` and pyright will be more important in practice? Not sure.

How we answer this question could have significant implications for how we implement this; the more special-cased the name lookup rules for stringified annotations are, the more it suggests we should not transparently try to parse them to AST in the parser.

</details>


---

_Label `red-knot` added by @carljm on 2024-10-17 16:41_

---

_Comment by @Viicos on 2024-10-18 11:47_

`Annotated` metadata also needs to be special cased, similar to `Literal` :)

---

_Comment by @carljm on 2024-10-18 17:54_

> `Annotated` metadata also needs to be special cased, similar to `Literal` :)

Thanks! Edited the description to reflect this.

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:23_

---

_Assigned to @dhruvmanila by @carljm on 2024-11-07 15:23_

---

_Referenced in [astral-sh/ruff#14151](../../astral-sh/ruff/pulls/14151.md) on 2024-11-13 18:12_

---

_Closed by @dhruvmanila on 2024-11-15 04:10_

---

_Closed by @dhruvmanila on 2024-11-15 04:10_

---
