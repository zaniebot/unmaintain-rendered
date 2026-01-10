```yaml
number: 582
title: "Invalid argument to `fields` from `attrs`"
type: issue
state: closed
author: my1e5
labels:
  - bug
  - Protocols
assignees: []
created_at: 2025-06-04T10:42:54Z
updated_at: 2025-08-18T20:38:20Z
url: https://github.com/astral-sh/ty/issues/582
synced_at: 2026-01-10T02:06:24Z
```

# Invalid argument to `fields` from `attrs`

---

_Issue opened by @my1e5 on 2025-06-04 10:42_

### Summary

When using `fields` from `attrs`, ty reports a `error[invalid-argument-type]`.

Similar to 
 * https://github.com/astral-sh/ty/issues/92

and related to 

* https://github.com/astral-sh/ty/issues/111.

## MRE

```py
from attrs import define, fields


@define
class Foo:
    x: int
    y: float
    z: str


for field in fields(Foo):
    print(field.name, field.type)
```

```
$ uvx --with attrs==25.3.0 ty check dev/foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-argument-type]: Argument to function `fields` is incorrect
  --> dev\foo.py:11:21
   |
11 | for field in fields(Foo):
   |                     ^^^ Expected `type[AttrsInstance]`, found `<class 'Foo'>`
12 |     print(field.name, field.type)
   |
info: Function defined here
   --> .venv\Lib\site-packages\attr\__init__.pyi:311:5
    |
309 |     unsafe_hash: bool | None = ...,
310 | ) -> Callable[[_C], _C]: ...
311 | def fields(cls: type[AttrsInstance]) -> Any: ...
    |     ^^^^^^ ------------------------ Parameter declared here
312 | def fields_dict(cls: type[AttrsInstance]) -> dict[str, Attribute[Any]]: ...
313 | def validate(inst: AttrsInstance) -> None: ...
    |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```
---
`AttrsInstance` is a protocol

https://github.com/python-attrs/attrs/blob/a6ae894aad9bc09edc7cdad8c416898784ceec9b/src/attr/__init__.pyi#L67
```py
# We subclass this here to keep the protocol's qualified name clean.
class AttrsInstance(AttrsInstance_, Protocol):
    pass
```

https://github.com/python-attrs/attrs/blob/a6ae894aad9bc09edc7cdad8c416898784ceec9b/src/attr/_typing_compat.pyi#L8

```py
from typing import Any, ClassVar, Protocol

# MYPY is a special constant in mypy which works the same way as `TYPE_CHECKING`.
MYPY = False

if MYPY:
    # A protocol to be able to statically accept an attrs class.
    class AttrsInstance_(Protocol):
        __attrs_attrs__: ClassVar[Any]

else:
    # For type checkers without plug-in support use an empty protocol that
    # will (hopefully) be combined into a union.
    class AttrsInstance_(Protocol):
        pass
```

### Version

ty 0.0.1-alpha.8 (c1337c962 2025-06-02)

---

_Comment by @sharkdp on 2025-06-04 11:45_

Interesting, thank you for reporting this. I think this should be fixed by modelling assignability of class literals `C` to `type[SomeProtocol]` if instances of `C` implement `SomeProtocol`?

MRE without attrs dependency:
```py
from typing import Protocol

class AttrsInstance(Protocol):
    pass

def fields(a: type[AttrsInstance]):
    pass

class C: ...

fields(C)
```

cc @AlexWaygood 

---

_Label `bug` added by @sharkdp on 2025-06-04 11:45_

---

_Label `Protocols` added by @sharkdp on 2025-06-04 11:45_

---

_Comment by @AlexWaygood on 2025-06-04 11:56_

> I think this should be fixed by modelling assignability of class literals `C` to `type[SomeProtocol]` if instances of `C` implement `SomeProtocol`?

Yes. I haven't done much work on protocol meta-types (and meta-protocols...) yet, but it's on my TODO list. Good to have an issue tracking it!

---

_Closed by @AlexWaygood on 2025-08-18 20:38_

---
