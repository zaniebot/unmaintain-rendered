```yaml
number: 1987
title: "Custom dataclasses with `__dataclass_{fields,params}__` fields are not considered `DataclassInstance`"
type: issue
state: closed
author: jeertmans
labels:
  - bug
  - dataclasses
  - library
assignees: []
created_at: 2025-12-17T08:46:35Z
updated_at: 2025-12-17T13:22:18Z
url: https://github.com/astral-sh/ty/issues/1987
synced_at: 2026-01-10T01:55:00Z
```

# Custom dataclasses with `__dataclass_{fields,params}__` fields are not considered `DataclassInstance`

---

_Issue opened by @jeertmans on 2025-12-17 08:46_

### Summary

Hi!

First of all, thank you very much for this tool!

I use [`equinox.Module`](https://docs.kidger.site/equinox/api/module/module/#equinox.Module) to create Python objects that are both dataclasses and PyTrees (to be compatible with JAX). However, it looks like running `ty check` emits a lot of errors that should not occur, at least from what I understood: a Python class is a `DataclassInstance` if it has both `__dataclass_fields__` and `__dataclass_params__` attributes. Another issue is that `ty` will raise an error about missing required arguments, whereas they are not missing.

I looked at other issues on this repo and couldn't find a similar one, but maybe I missed it.

FYI, I tried to used the playground but I don't see how we can include external dependencies, and the `equinox` module is too complex to be included manually.

## MWE (`check.py`)

```python
# /// script
# requires-python = ">=3.11"
# dependencies = [
#     "equinox",
# ]
# ///
from dataclasses import dataclass, replace, asdict, field

import equinox as eqx


@dataclass
class MyBuiltinDataClass(eqx.Module):
    a: int
    b: float
    c: str = field(default="default_value")


class MyEqxDataClass(eqx.Module):
    a: int
    b: float
    c: str = eqx.field(default="default_value")


m = MyBuiltinDataClass(a=1, b=2.0)

print(m)
print(replace(m, b=3.5))
print(asdict(m))
print(hasattr(m, "__dataclass_fields__"))  # True
print(hasattr(m, "__dataclass_params__"))  # True

m = MyEqxDataClass(a=1, b=2.0)

print(m)
print(replace(m, b=3.5))
print(asdict(m))
print(hasattr(m, "__dataclass_fields__"))  # True
print(hasattr(m, "__dataclass_params__"))  # True
```

## `uv run python check.py`'s output

```
MyBuiltinDataClass(a=1, b=2.0, c='default_value')
MyBuiltinDataClass(a=1, b=3.5, c='default_value')
{'a': 1, 'b': 2.0, 'c': 'default_value'}
True
True
MyEqxDataClass(a=1, b=2.0)
MyEqxDataClass(a=1, b=3.5)
{'a': 1, 'b': 2.0, 'c': 'default_value'}
True
True
```

## `ty check check.py`'s output

```
error[missing-argument]: No argument provided for required parameter `c`
  --> check.py:26:5
   |
24 | print(hasattr(m, "__dataclass_params__"))  # True
25 |
26 | m = MyEqxDataClass(a=1, b=2.0)
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^
27 |
28 | print(m)
   |
info: rule `missing-argument` is enabled by default

error[invalid-argument-type]: Argument to function `replace` is incorrect
  --> check.py:29:15
   |
28 | print(m)
29 | print(replace(m, b=3.5))
   |               ^ Argument type `MyEqxDataClass` does not satisfy upper bound `DataclassInstance` of type variable `_DataclassT`
30 | print(asdict(m))
31 | print(hasattr(m, "__dataclass_fields__"))  # True
   |
info: Type variable defined here
  --> stdlib/dataclasses.pyi:32:1
   |
30 |     __all__ += ["KW_ONLY"]
31 |
32 | _DataclassT = TypeVar("_DataclassT", bound=DataclassInstance)
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
33 |
34 | @type_check_only
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `asdict` is incorrect
  --> check.py:30:14
   |
28 | print(m)
29 | print(replace(m, b=3.5))
30 | print(asdict(m))
   |              ^ Expected `DataclassInstance`, found `MyEqxDataClass`
31 | print(hasattr(m, "__dataclass_fields__"))  # True
32 | print(hasattr(m, "__dataclass_params__"))  # True
   |
info: Matching overload defined here
  --> stdlib/dataclasses.pyi:67:5
   |
66 | @overload
67 | def asdict(obj: DataclassInstance) -> dict[str, Any]:
   |     ^^^^^^ ---------------------- Parameter declared here
68 |     """Return the fields of a dataclass instance as a new dictionary mapping
69 |     field names to field values.
   |
info: Non-matching overloads for function `asdict`:
info:   (obj: DataclassInstance, *, dict_factory: (list[tuple[str, Any]], /) -> _T@asdict) -> _T@asdict
info: rule `invalid-argument-type` is enabled by default

Found 3 diagnostics
```

### Version

ty 0.0.2

---

_Label `dataclasses` added by @sharkdp on 2025-12-17 09:14_

---

_Label `library` added by @sharkdp on 2025-12-17 09:14_

---

_Label `bug` added by @sharkdp on 2025-12-17 09:18_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-17 09:18_

---

_Comment by @sharkdp on 2025-12-17 09:23_

Thank you for reporting this.

`equinox.Module` is a [metaclass-based `dataclass_transform`er](https://github.com/patrick-kidger/equinox/blob/2540dc3f54aa962e001ba826ab14c6f08bf64a6f/equinox/_module/_module.py#L443). We should be recognizing this. And in fact we do (partially?) recognize `MyEqxDataClass` as a dataclass. For example, `reveal_type(MyEqxDataClass.__init__)` yields `(self: MyEqxDataClass, a: int, b: int | float, c: str) -> None`. But other parts are not working. The `c` parameter should have a default (apparently we do not recognize the field specifier [declared here](https://github.com/patrick-kidger/equinox/blob/2540dc3f54aa962e001ba826ab14c6f08bf64a6f/equinox/_module/_module.py#L269). And the special dunder attributes should also be available.

---

_Assigned to @sharkdp by @sharkdp on 2025-12-17 10:10_

---

_Comment by @jeertmans on 2025-12-17 10:38_

Thanks for reply @sharkdp and also for the upcoming patch! 🔥 (https://github.com/astral-sh/ruff/pull/22018 👀 )

---

_Closed by @sharkdp on 2025-12-17 13:22_

---
