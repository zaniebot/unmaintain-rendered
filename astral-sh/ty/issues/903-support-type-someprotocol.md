```yaml
number: 903
title: "Support `type[SomeProtocol]`"
type: issue
state: open
author: tpgillam
labels:
  - type properties
  - Protocols
assignees: []
created_at: 2025-07-27T13:55:46Z
updated_at: 2025-10-17T09:51:08Z
url: https://github.com/astral-sh/ty/issues/903
synced_at: 2026-01-12T15:54:24Z
```

# Support `type[SomeProtocol]`

---

_@tpgillam_

### Question

With ty 0.0.1-alpha16, consider the following:

```python
from __future__ import annotations

import dataclasses
import typing

if typing.TYPE_CHECKING:
    import _typeshed


def moo1(cls: _typeshed.DataclassInstance) -> None:
    print(cls)


def moo2(cls: type[_typeshed.DataclassInstance]) -> None:
    print(cls)


def moo3[T: _typeshed.DataclassInstance](cls: type[T]) -> None:
    print(cls)


@dataclasses.dataclass
class A:
    a: int


moo1(A(1))  # ty happy
moo2(A)     # ty unhappy
moo3(A)     # ty happy
```

Then `uvx ty check` will give the following output:

```
error[invalid-argument-type]: Argument to function `moo2` is incorrect
  --> moo2.py:28:6
   |
27 | moo1(A(1))
28 | moo2(A)
   |      ^ Expected `type[DataclassInstance]`, found `<class 'A'>`
29 | moo3(A)
   |
info: Function defined here
  --> moo2.py:14:5
   |
14 | def moo2(cls: type[_typeshed.DataclassInstance]) -> None:
   |     ^^^^ -------------------------------------- Parameter declared here
15 |     print(cls)
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

Is this expected / correct behaviour? pyright will accept all the calls. 

Since `DataclassInstance` is a protocol it seems plausible that `type[DataclassInstance]` might be poorly defined (I don't know the ins-and-outs of the typing spec) ... but if that _were_ the case then I'm mildly surprised that ty doesn't complain about:

```python
from collections.abc import Sequence

def moo4(a: type[Sequence]) -> None:
    print(a)

moo4(list)
```

---

_Label `question` added by @tpgillam on 2025-07-27 13:55_

---

_Label `question` removed by @sharkdp on 2025-07-28 07:25_

---

_Label `type properties` added by @sharkdp on 2025-07-28 07:25_

---

_Label `Protocols` added by @sharkdp on 2025-07-28 07:25_

---

_Comment by @sharkdp on 2025-07-28 07:27_

Thank you for reporting this. This is not related to `DataclassInstance` in particular, it seems:

```py
from typing import Protocol


class P(Protocol):
    x: int


class C:
    x: int = 1


p1: P = C()
p2: type[P] = C   # invalid-assignment
```

This looks like a bug to me. @AlexWaygood will be able to tell more.

---

_Renamed from "Behaviour of `type[DataclassInstance]` and similar" to "Support `type[SomeProtocol]`" by @carljm on 2025-08-05 02:43_

---

_Comment by @carljm on 2025-08-05 02:46_

I think it is a known issue that this isn't supported yet. It's a bit of a tricky feature, because it's not well-defined in the Python type system what attributes of a protocol should be considered/assumed to necessarily exist on the meta-type of some type that satisfies the protocol. For example, people also want module objects to satisfy protocols (see #931), but the meta-type of a module object (which is almost always `ModuleType`) almost certainly doesn't include whatever attributes are defined on the protocol.

But `DataclassInstance` defines an attribute with `ClassVar`, making it explicit that it must exist on the meta-type. So that case at least should be made to work.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:10_

---

_Added to milestone `GA` by @AlexWaygood on 2025-10-17 09:51_

---
