---
number: 1444
title: "`_MISSING_TYPE` is not callable for `@<field>.default` in `attrs` class"
type: issue
state: open
author: my1e5
labels:
  - library
assignees: []
created_at: 2025-10-27T11:06:09Z
updated_at: 2026-01-09T03:13:10Z
url: https://github.com/astral-sh/ty/issues/1444
synced_at: 2026-01-10T01:48:23Z
---

# `_MISSING_TYPE` is not callable for `@<field>.default` in `attrs` class

---

_Issue opened by @my1e5 on 2025-10-27 11:06_

### Summary

`attrs` allows you to define a default factory by decorating a function. Since ty `0.0.1a24` this gives an error: `error[call-non-callable]: Object of type _MISSING_TYPE is not callable`.

## MRE

```py
from attrs import define, field


@define
class Foo:
    x: int = 1
    y: int = field()

    @y.default
    def _default_y(self) -> int:
        return self.x + 1
```
## ty `0.0.1a24`
```
$ uvx --with attrs==25.4.0 --with ty==0.0.1a24 ty check dev/attrs_ty.py
Checking ------------------------------------------------------------ 1/1 files
error[call-non-callable]: Object of type `_MISSING_TYPE` is not callable
  --> dev\attrs_ty.py:9:5
   |
 7 |     y: int = field()
 8 |
 9 |     @y.default
   |     ^^^^^^^^^^
10 |     def _default_y(self) -> int:
11 |         return self.x + 1
   |
info: Union variant `_MISSING_TYPE` is incompatible with this call site
info: Attempted to call union type `Unknown | _MISSING_TYPE`
info: rule `call-non-callable` is enabled by default

Found 1 diagnostic
```
## ty `0.0.1a23`
```
$ uvx --with attrs==25.4.0 --with ty==0.0.1a23 ty check dev/attrs_ty.py
Checking ------------------------------------------------------------ 1/1 files
All checks passed!
```


On a similar theme but possibly not related:
* https://github.com/astral-sh/ty/issues/267

### Version

ty 0.0.1-alpha.24 (1fee7da8b 2025-10-23)

---

_Renamed from "`_MISSING_TYPE` is not callable for `@<field>.default in `attrs` class" to "`_MISSING_TYPE` is not callable for `@<field>.default` in `attrs` class" by @my1e5 on 2025-10-27 11:06_

---

_Label `library` added by @AlexWaygood on 2025-10-27 11:13_

---

_Comment by @sharkdp on 2025-10-27 12:56_

Thank you for reporting this. This is due to the typeshed annotation of `Field.default` here:

https://github.com/python/typeshed/blob/16f766b754405004471af20eeb9a5cf8dea05b44/stdlib/dataclasses.pyi#L206

It's unclear to me how this should work without dedicated attrs support (mypy seems to support this [via a plugin](https://github.com/python/mypy/blob/master/mypy/plugins/attrs.py#L575-L602)).

---

_Comment by @carljm on 2025-10-29 20:09_

It seems to me like stdlib `dataclasses.Field` here should not be involved at all? Because this is using `attrs.field` function, which is annotated to return `Any` in the cases where no default is given upfront: https://github.com/python-attrs/attrs/blob/main/src/attrs/__init__.pyi#L86

So it seems like we are making a wrong assumption that all dataclass-transform fields are always of type `dataclasses.Field`, which is not necessarily true.

---

_Comment by @carljm on 2025-10-29 20:45_

But (after discussion with @sharkdp), type annotations for field-specifier functions are annotated (unfortunately) as returning `Any` or their default type, not as the actual field object they return. So type checkers have no way to understand the characteristics of the actual returned field object. And other type checkers (without a dedicated plugin) generally don't even try to model that there _is_ a field object/type at all (e.g. `dataclasses.Field`), they just model the field always as its value type (e.g. `int`). So pyright also fails on the OP example here, with `error: Cannot access attribute "default" for class "int"`. If mypy succeeds on this, it's only due to a plugin.

So I think this is something that we would have to support via dedicated built-in `attrs` support.

---

_Added to milestone `Stable` by @carljm on 2026-01-09 03:13_

---
