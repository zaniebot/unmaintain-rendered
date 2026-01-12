```yaml
number: 92
title: "Invalid argument to `fields` from `dataclasses`"
type: issue
state: closed
author: my1e5
labels:
  - bug
  - help wanted
  - dataclasses
assignees: []
created_at: 2025-05-07T13:36:54Z
updated_at: 2025-05-15T13:02:22Z
url: https://github.com/astral-sh/ty/issues/92
synced_at: 2026-01-12T15:54:22Z
```

# Invalid argument to `fields` from `dataclasses`

---

_@my1e5_

### Summary

When using `fields` from `dataclasses`, ty gives a `lint:invalid-argument-type` error.

https://play.ty.dev/331a08e9-3adf-41f4-a4a5-483a964a1b64

```py
from dataclasses import dataclass, fields


@dataclass
class Foo:
    x: int
    y: float
    z: str


for field in fields(Foo):
    print(field.name, field.type)
```

```
$ uvx ty check foo.py 
error: lint:invalid-argument-type: Argument to this function is incorrect
  --> foo.py:11:21
   |
11 | for field in fields(Foo):
   |                     ^^^ Expected `DataclassInstance`, found `Literal[Foo]`
12 |     print(field.name, field.type)
   |
info: Function defined here
   --> stdlib\dataclasses.pyi:220:5
    |
218 |     ) -> Any: ...
219 |
220 | def fields(class_or_instance: DataclassInstance | type[DataclassInstance]) -> tuple[Field[Any], ...]: ...
    |     ^^^^^^ -------------------------------------------------------------- Parameter declared here
221 |
222 | # HACK: `obj: Never` typing matches if object argument is using `Any` type.
    |
```

There is a similar error for `fields` from `attrs`:

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
$ uvx ty check foo.py
error: lint:invalid-argument-type: Argument to this function is incorrect
  --> foo.py:11:21
   |
11 | for field in fields(Foo):
   |                     ^^^ Expected `type[AttrsInstance]`, found `Literal[Foo]`
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

Found 1 diagnostic
```


### Version

ty 0.0.0-alpha.5 (ff9000864 2025-05-06)

---

_Label `bug` added by @sharkdp on 2025-05-07 13:39_

---

_Comment by @sharkdp on 2025-05-07 13:40_

`DataclassInstance` is a protocol:

```pyi
class DataclassInstance(Protocol):
    __dataclass_fields__: ClassVar[dict[str, Field[Any]]]
```

We can probably fix this rather easily by adding a synthesized `__dataclass_fields__` member to dataclasses (by adding a new case to `Type::member_lookup_with_policy` or `Type::find_name_in_mro_with_policy`).

---

_Label `help wanted` added by @sharkdp on 2025-05-07 13:41_

---

_Renamed from "[ty] Invalid argument to `fields` from `dataclasses`" to "Invalid argument to `fields` from `dataclasses`" by @MichaReiser on 2025-05-07 15:24_

---

_Comment by @abhijeetbodas2001 on 2025-05-07 16:37_

@sharkdp can I work on this?

---

_Comment by @AlexWaygood on 2025-05-07 16:39_

@abhijeetbodas2001 go for it!

---

_Assigned to @abhijeetbodas2001 by @AlexWaygood on 2025-05-07 16:39_

---

_Comment by @my1e5 on 2025-05-08 11:09_

Related 
* https://github.com/astral-sh/ty/issues/111

---

_Label `dataclasses` added by @AlexWaygood on 2025-05-10 17:59_

---

_Closed by @sharkdp on 2025-05-13 08:31_

---

_Comment by @abhijeetbodas2001 on 2025-05-15 13:02_

See also: [#18115](https://github.com/astral-sh/ruff/pull/18115)

---
