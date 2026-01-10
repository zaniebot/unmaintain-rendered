```yaml
number: 2370
title: "Class attribute returns Unknown | type"
type: issue
state: closed
author: merlinz01
labels: []
assignees: []
created_at: 2026-01-06T19:34:03Z
updated_at: 2026-01-06T20:27:52Z
url: https://github.com/astral-sh/ty/issues/2370
synced_at: 2026-01-10T01:56:41Z
```

# Class attribute returns Unknown | type

---

_Issue opened by @merlinz01 on 2026-01-06 19:34_

### Summary

Ty says that a simple `int` attribute defined on a class is `Unknown | int`. I was running into this with the Tortoise ORM, but it is reproducible with much simpler code.

```python
# testit.py

from typing import reveal_type


class MyClass:
    attr = 42


reveal_type(MyClass.attr)
reveal_type(MyClass().attr)
```

```sh
$ ty check testit.py
info[revealed-type]: Revealed type
 --> backend/testit.py:8:13
  |
8 | reveal_type(MyClass.attr)
  |             ^^^^^^^^^^^^ `Unknown | Literal[42]`
9 | reveal_type(MyClass().attr)
  |

info[revealed-type]: Revealed type
 --> backend/testit.py:9:13
  |
8 | reveal_type(MyClass.attr)
9 | reveal_type(MyClass().attr)
  |             ^^^^^^^^^^^^^^ `Unknown | Literal[42]`
  |

Found 2 diagnostics
```

Using `reveal_type(attr)` inside the class definition returns `Literal[42]` as expected.

Possibly related: #312

### Version

ty 0.0.9

---

_Comment by @AlexWaygood on 2026-01-06 20:27_

Hey, thanks for the report. This is an area where ty (currently) has different behaviour to other type checkers. There are multiple types that ty _could_ infer for this attribute: `Literal[42]`, `int`, `Literal[42] | Unknown`, `int | Unknown`, `int | None`, `object`, `typing.SupportsIndex`... you could go on for a long time. Exactly what the user _intended_ the type to be for all assignments to this attribute is ambiguous without a type annotation.

Note that just because it is inferred as `int | Unknown` doesn't mean that you lose all type safety! If you try doing `MyClass.attr + "foo"`, ty will report an error, because that operation is not valid for all elements in the union.

See also our FAQ [here](https://docs.astral.sh/ty/reference/typing-faq/#what-is-the-unknown-type-and-when-does-it-appear). Note that we plan on at least making this behaviour configurable, and we may change our default: see https://github.com/astral-sh/ty/issues/1240

---

_Closed by @AlexWaygood on 2026-01-06 20:27_

---
