```yaml
number: 7695
title: "`enum.global_enum` members are not added to global scope"
type: issue
state: open
author: petrem
labels:
  - bug
assignees: []
created_at: 2023-09-28T10:38:41Z
updated_at: 2025-02-03T19:23:31Z
url: https://github.com/astral-sh/ruff/issues/7695
synced_at: 2026-01-12T15:54:47Z
```

# `enum.global_enum` members are not added to global scope

---

_@petrem_

Consider this `foo.py`:
```python
import enum

@enum.global_enum
class Foo(enum.Enum):
    A = 1

print(A)
```

While the code works
```bash
$ python foo.py
A
```

ruff complains
```bash
$ ruff --version
ruff 0.0.291

$ ruff foo.py

foo.py:7:7: F821 Undefined name `A`
Found 1 error.
```

`enum.global_enum` was introduced in Python 3.11.

Thank you for a super-cool-tool! I'm tempted to learn rust just to be be able to contribute :D.

---

_Comment by @zanieb on 2023-09-28 13:56_

Honestly, I had no idea `global_enum` existed! We'll need to add special handling this while constructing our semantic model.

Thank you :) learn some Rust!

---

_Label `bug` added by @zanieb on 2023-09-28 13:56_

---

_Renamed from "enum.global_enum is not honoured" to "`enum.global_enum` members are not added to global scope" by @zanieb on 2023-09-28 13:57_

---

_Comment by @petrem on 2023-10-01 12:03_

> Honestly, I had no idea global_enum existed! 

Turns out most other python tools fail in the same way, at the moment.

> learn some Rust!
Alas, so much to do, so little time (but I got ~the~ a book already).

Thank you!

---

_Comment by @goodmami on 2023-11-22 04:15_

For Python versions prior to 3.11, there is [another idiom](https://stackoverflow.com/a/28130684) for putting Enums in the global namespace:

```python
globals().update(Foo.__members__)
```

Ruff complains with an F821 on enums added this way, too.

This seems like a bigger issue than just enums, though, and modifying the dict returned by `globals()` is allowed by Python ([link](https://stackoverflow.com/a/5958992)), unlike for `locals()`. Shall I raise a separate issue for it?

---

_Comment by @MichaReiser on 2023-11-27 03:18_

@goodmami this pattern is probably difficult to support for static analysis tools. E.g. what if the `update` call is inside a function or an `if`. Should `ruff` assume that the members are part of the globals or not? 

Do you know if other static-analysis tools support the above pattern? How common is this idiom in your code? Could you declare the globals in the configuration file instead?

---

_Comment by @goodmami on 2023-11-27 04:47_

> this pattern is probably difficult to support for static analysis tools.

@MichaReiser Yeah, I was afraid of that.

> Do you know if other static-analysis tools support the above pattern?

Both flake8 and mypy complain about the undeclared names, and it doesn't look like pyright will handle this pattern either (https://github.com/microsoft/pyright/issues/2485).

> How common is this idiom in your code?

Until now, unused. I was looking up recommended patterns for making Enum members into module-level constants and came across the SO posts by the author of the enum library and a CPython core developer. Honestly, I was a bit surprised at the recommendations and *not* surprised that static analysis tools were unable to deal with it.

> Could you declare the globals in the configuration file instead?

What setting allows me to declare module-level constants? If this would require me to list them out, I might as well just have:

```python
A = Foo.A
B = Foo.B
...
```

in my module instead.

In any case, I think I'll just ignore the `globals().update()` trick and wait for the `@enum.global_enum` decorator in Python 3.11.

---

_Label `help wanted` added by @zanieb on 2023-11-27 15:35_

---

_Label `help wanted` removed by @MichaReiser on 2025-02-03 19:23_

---
