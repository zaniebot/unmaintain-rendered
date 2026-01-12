```yaml
number: 7060
title: "Failed to create fix for SuppressibleException: Unable to use existing symbol due to incompatible context"
type: issue
state: closed
author: qarmin
labels:
  - fuzzer
assignees: []
created_at: 2023-09-02T06:23:15Z
updated_at: 2023-09-02T11:24:07Z
url: https://github.com/astral-sh/ruff/issues/7060
synced_at: 2026-01-12T15:54:46Z
```

# Failed to create fix for SuppressibleException: Unable to use existing symbol due to incompatible context

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)

```
ruff  *.py --select ALL,NURSERY --no-cache --fix
```

file content:
```
# Copyright (c) 2021-present davfsa
import typing
import multidict
from mable import errors
if typing.TYPE_CHECKING:
    import contextlib
    T_co = typing.TypeVar("T_co", covariant=True)
    T = typing.TypeVar("T")
_StringMapBuilderArg = typing.Union[
    typing.Mapping[str, str], multidict.MultiMapping[str], typing.Iterable[typing.Tuple[str, str]]
]
try:
    import orjson
    default_json_loads = orjson.loads
except ModuleNotFoundError:
    _json_separators = (",", ":")
def cast_variants_array(cast: typing.Callable[[T_co], T], raw_values: typing.Iterable[T_co], /) -> typing.List[T]:
    results: typing.List[T] = []
    for value in raw_values:
        try:
            results.append(cast(value))
        except errors.UnrecognisedEntityError:
            pass
```

error:
```
[error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Attributes3841.py`, the rule codes B009, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

Attributes3841.py:1:1: T201 `print` found
Attributes3841.py:1:1: D100 Missing docstring in public module
Attributes3841.py:1:1: CPY001 Missing copyright notice at top of file
Attributes3841.py:1:7: B009 Do not call `getattr` with a constant attribute value. It is not any safer than normal property access.
](error: Failed to create fix for SuppressibleException: Unable to use existing symbol due to incompatible context)
```

[4879827.py.zip](https://github.com/astral-sh/ruff/files/12503293/4879827.py.zip)

This error is only print once, when fixing file again problem dissapears


---

_Comment by @charliermarsh on 2023-09-02 11:24_

Same as #6842.

---

_Closed by @charliermarsh on 2023-09-02 11:24_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-02 11:24_

---
