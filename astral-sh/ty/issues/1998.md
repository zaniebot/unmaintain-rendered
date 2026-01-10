```yaml
number: 1998
title: ty hangs on combination of self-ref Union, dataclass and Protocol
type: issue
state: open
author: sinon
labels:
  - hang
assignees: []
created_at: 2025-12-17T10:40:24Z
updated_at: 2025-12-26T12:25:35Z
url: https://github.com/astral-sh/ty/issues/1998
synced_at: 2026-01-10T01:56:41Z
```

# ty hangs on combination of self-ref Union, dataclass and Protocol

---

_Issue opened by @sinon on 2025-12-17 10:40_

### Summary

An attempt at an MRE for a hang we are seeing on one of our repos:
Run with: `uv tool run ty check mre.py`
```py
from __future__ import annotations  # removing this cause it not to hang
import dataclasses

from typing import (
    Protocol,
    Union,
)


@runtime_checkable
class Traceable(Protocol):
    """Protocol for objects that provide custom trace representations."""

    def trace_repr(self) -> TraceableValue:
        """Return a dictionary representation suitable for tracing."""
        ...


TraceableValue = Union[
    Traceable,
    bool,  # removing bool causes it to not hang
    # One of the following self-referential types are needed to hang
    # list["TraceableValue"],
    # dict[str, "TraceableValue"],
    tuple["TraceableValue", ...],
]


@dataclasses.dataclass
class FilledOutfit:
    def trace_repr(self) -> TraceableValue:
        return False

```

No play.ty.dev link as it causes a fun failure state:

<img width="1251" height="893" alt="Image" src="https://github.com/user-attachments/assets/b4917ca3-5953-463b-9ac6-10d7e0a30cad" />

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @judahrand on 2025-12-17 10:41_

Slightly simpler reproduction:

```python
from typing import Protocol, Union

SomeUnion = Union[
    "SomeProtocol",
    bool,
    # Any of these cause a hang
    list["SomeUnion"],
    # dict[str, "SomeUnion"],
    # tuple["SomeUnion", ...],
]


class SomeProtocol(Protocol):
    def some_method(self) -> SomeUnion: ...


class SomeImplementation:
    def some_method(self) -> SomeUnion:
        return True
```

---

_Label `hang` added by @MichaReiser on 2025-12-17 10:42_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-17 10:42_

---

_Comment by @sinon on 2025-12-23 14:31_

This has slightly improved on `main`, the original snippet will now paste into the playground and not go 💥 even points out that I was missing the `runtime_checkable` import. Though un-commenting the `dict[str, "TraceableValue"]` or `list["TraceableValue"]` part of the union in addition to the `tuple` will cause it to hang again. 

https://play.ty.dev/fb84d53a-5c1d-4aaa-be1f-45c601e3e29f - still very laggy but works.



Still the weirdest part is the when `bool` isn't present in the `Union` I can safely have all the self-referential types so the below is fine until you uncomment the `bool` (and no lag while typing unlike the above playground link)
https://play.ty.dev/69e45921-a8af-41da-ac83-dc297dff6425
```py
import dataclasses

from typing import (
    Protocol,
    Union,
)


class Traceable(Protocol):
    """Protocol for objects that provide custom trace representations."""

    def trace_repr(self) -> TraceableValue:
        """Return a dictionary representation suitable for tracing."""
        ...


TraceableValue = Union[
    Traceable,
    #bool,  # uncomment to break 
    list["TraceableValue"],
    dict[str, "TraceableValue"],
    tuple["TraceableValue", ...],
]


@dataclasses.dataclass
class FilledOutfit:
    def trace_repr(self) -> TraceableValue:
        return ()
```

---

_Removed from milestone `Stable` by @carljm on 2025-12-23 15:52_

---

_Added to milestone `M1` by @carljm on 2025-12-23 15:52_

---

_Assigned to @mtshiba by @mtshiba on 2025-12-25 17:27_

---

_Comment by @mtshiba on 2025-12-26 12:25_

This should be resolved with #1738.

---
