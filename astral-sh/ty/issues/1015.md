```yaml
number: 1015
title: "Support `Annotated` types as instance attributes (and pydantic constrained types)"
type: issue
state: closed
author: mvernacc
labels: []
assignees: []
created_at: 2025-08-15T20:23:28Z
updated_at: 2025-08-16T13:28:04Z
url: https://github.com/astral-sh/ty/issues/1015
synced_at: 2026-01-10T02:06:24Z
```

# Support `Annotated` types as instance attributes (and pydantic constrained types)

---

_Issue opened by @mvernacc on 2025-08-15 20:23_

### Summary

I would like ty to support type inference on class instance attributes declared with `Annotated`. Particularly, I'm interested in correct type inference for fields of pydantic models (pydantic makes heavy use of `Annotated` on model fields).

## Simple example

The playground below shows that:
- ty correctly infers the type `float` for an annotated variable (outside a class)
- ~~ty incorrectly infers type `int | float` for an annotated instance attribute~~
  - [Edit] As @carljm pointed out below, this is actually correct due to a nuance in the python type system.

https://play.ty.dev/7dd54204-0cf0-4076-8ca0-d5e421ad66c3

## Example with Pydantic

The below example shows a pydanic `BaseModel` subclass. One field uses `Annotated` directly. Another field uses a [pydantic constrained number type](https://docs.pydantic.dev/2.3/usage/types/number_types/#constrained-floats) which uses `Annotated` under the hood.

```python
from typing import Annotated, reveal_type

from pydantic import BaseModel, PositiveFloat


class MyModel(BaseModel):
    annotated: Annotated[float, "test annotation"]
    pydantic_constrained: PositiveFloat

m = MyModel(annotated=2.0, pydantic_constrained=1.0)
reveal_type(m.annotated)
reveal_type(m.pydantic_constrained)
```

`ty` does not infer the types for ~~these fields correctly~~ [Edit: the first field's type is correct]:

```
> ty check demo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                  info[revealed-type]: Revealed type
  --> demo.py:11:13
   |
10 | m = MyModel(annotated=2.0, pydantic_constrained=1.0)
11 | reveal_type(m.annotated)
   |             ^^^^^^^^^^^ `int | float`
12 | reveal_type(m.pydantic_constrained)
   |

info[revealed-type]: Revealed type
  --> scripts/ty_demo.py:12:13
   |
10 | m = MyModel(annotated=2.0, pydantic_constrained=1.0)
11 | reveal_type(m.annotated)
12 | reveal_type(m.pydantic_constrained)
   |             ^^^^^^^^^^^^^^^^^^^^^^ `Unknown`
   |

Found 2 diagnostics
```

For comparison, `pyright` infers type `float` for both fields. This is the behavior I desire:

```
  demo.py:11:13 - information: Type of "m.annotated" is "float"
  demo.py:12:13 - information: Type of "m.pydantic_constrained" is "float"
0 errors, 0 warnings, 2 informations 
```

Possibly related issues:
- https://github.com/astral-sh/ty/issues/111
- https://github.com/astral-sh/ty/issues/504


p.s. Thank you for your work on ty! I love its speed and am looking forward to using it more. Better support for pydantic models would allow my team and I to adopt ty on more of our projects :)

### Version

ty 0.0.1-alpha.18

---

_Comment by @carljm on 2025-08-15 20:54_

Thanks for the report!

Our inference of `float | int` for the field named `annotated` is the same as pyright's inference of `float`. There's an [awkward special case](https://typing.python.org/en/latest/spec/special-types.html#special-cases-for-float-and-complex) in the Python type system where an annotation of `float` actually means `float | int` (`float` is not a supertype of `int`). Other type checkers attempt to "hide" this special case by also displaying the type `float | int` as just `float`. We feel that hiding the behavior this way just leads to even more confusing behavior in cases of type narrowing, so we display the full actual type. So when we say `float | int` here, that is the same type (with the same behavior) as pyright means by `float`; it's the direct interpretation of your `float` annotation.

The issue with `PositiveFloat` is a known limitation in our handling of implicit (legacy) type aliases; it should be fixed soon.

---

_Closed by @carljm on 2025-08-15 20:54_

---

_Comment by @mvernacc on 2025-08-16 13:28_


> the type system contains a straightforward shortcut: when an argument is annotated as having type `float`, an argument of type `int` is acceptable

TIL, thanks for pointing this out @carljm . It makes sense for ty to not hide this now that I know about it.

I'll watch for #221 to be resolved :)

---
