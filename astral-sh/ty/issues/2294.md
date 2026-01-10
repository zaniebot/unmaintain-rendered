---
number: 2294
title: Type expression with string literal treated as the object it represents.
type: issue
state: closed
author: inoa-jboliveira
labels: []
assignees: []
created_at: 2025-12-31T19:18:16Z
updated_at: 2026-01-06T01:27:21Z
url: https://github.com/astral-sh/ty/issues/2294
synced_at: 2026-01-10T01:51:14Z
---

# Type expression with string literal treated as the object it represents.

---

_Issue opened by @inoa-jboliveira on 2025-12-31 19:18_

### Summary

I have this annotated int type in an application where it just records the name of the actual list of ints (it has some metadata of each number, so not a literal).

```python
from typing import Annotated

foo: Annotated[int, 'FOO']

class ChoiceType:
    def __class_getitem__(cls, choice_name: str):
        return Annotated[int, choice_name]

foo: ChoiceType['FOO']
```

```shell
$ uvx ty check
error[unresolved-reference]: Name `FOO` used when not defined
 --> foo.py:9:18
  |
7 |         return Annotated[int, choice_name]
8 |
9 | foo: ChoiceType['FOO']
  |                  ^^^^^
  |
info: rule `unresolved-reference` is enabled by default

Found 1 diagnostic
```

If I import the name, it will complain that such object shall not be used in a type literal or whatever.

The point is. Why is it trying to convert a literal string into the value it represents? 

I understand this an edge case, but I was hoping these 2 were equivalent

```python
foo: Annotated[int, 'FOO']
foo: ChoiceType['FOO']
``` 

The 'FOO' from Annotated is treated as a literal

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Comment by @carljm on 2026-01-06 01:27_

Thanks for trying ty!

The syntax of type annotations is [quite constrained](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions); the type system won't let you put arbitrary dynamic code into a type annotation (although runtime will, mostly). No other type checker handles this in the way you are hoping for, either.

In a type annotation, subscript syntax always and only means a generic type with a type argument (or a known special form, like `Annotated`, `Literal`, etc), but `ChoiceType` is not a known special form and not a generic type (it doesn't inherit `typing.Generic` or use PEP 695 `class ChoiceType[T]` syntax).

A string in a type expression means a [string annotation](https://typing.python.org/en/latest/spec/annotations.html#string-annotations): this is the old (pre Python 3.14) way to enable forward references. This is why type checkers interpret the `"FOO"` in your annotation as a reference to something named `FOO`.

So ty is working as expected here -- the Python static type system just isn't designed to do what you are asking it to do.

---

_Closed by @carljm on 2026-01-06 01:27_

---
