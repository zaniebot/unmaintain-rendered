```yaml
number: 1588
title: Large enum takes very long to type check.
type: issue
state: open
author: MichaReiser
labels:
  - performance
  - enums
assignees: []
created_at: 2025-11-19T07:41:48Z
updated_at: 2026-01-09T13:43:55Z
url: https://github.com/astral-sh/ty/issues/1588
synced_at: 2026-01-12T15:54:25Z
```

# Large enum takes very long to type check.

---

_@MichaReiser_

Type checking the following file takes minutes (730s and counting) on my machine

https://gist.github.com/MichaReiser/c2ba46218ce8807356db6c6fa7d0b49e

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-19 07:41_

---

_Label `performance` added by @MichaReiser on 2025-11-19 07:41_

---

_Comment by @sharkdp on 2025-11-19 07:58_

I haven't investigated, but type-checking this code requires building large union and intersection types. We probably need the same optimization that we already implemented for int/str/bytes literals, see [this comment](https://github.com/astral-sh/ruff/blob/c4a47d1a901e481580d5ab71484376ff2479c8fb/crates/ty_python_semantic/src/types/builder.rs#L31).

---

_Comment by @MichaReiser on 2025-11-19 08:12_

Yeah, we spent an awful long time in union building

<img width="1777" height="769" alt="Image" src="https://github.com/user-attachments/assets/3569990e-b6d3-4c9c-ab1d-13dc56922661" />

Profile https://share.firefox.dev/49KBn59

---

_Comment by @ido123net on 2025-12-23 11:45_

I had a more "minimal" reproduce in #2183 

Pasting it here in case it will help solving this issue.

```python
from enum import Enum, auto


class SomeEnum(Enum):
    A1 = auto()
    A2 = auto()
    A3 = auto()
    A4 = auto()
    A5 = auto()
    A6 = auto()
    A7 = auto()
    A8 = auto()
    A9 = auto()
    A10 = auto()

    B1 = auto()
    B2 = auto()
    B3 = auto()
    B4 = auto()
    B5 = auto()
    B6 = auto()
    B7 = auto()
    B8 = auto()
    B9 = auto()
    B10 = auto()

    C1 = auto()
    C2 = auto()
    C3 = auto()
    C4 = auto()
    C5 = auto()
    C6 = auto()
    C7 = auto()
    C8 = auto()
    C9 = auto()
    C10 = auto()

    D1 = auto()
    D2 = auto()
    D3 = auto()
    D4 = auto()
    D5 = auto()
    D6 = auto()
    D7 = auto()
    D8 = auto()
    D9 = auto()
    D10 = auto()

    E1 = auto()
    E2 = auto()
    E3 = auto()
    E4 = auto()
    E5 = auto()
    E6 = auto()
    E7 = auto()
    E8 = auto()
    E9 = auto()
    E10 = auto()

    F1 = auto()
    F2 = auto()
    F3 = auto()
    F4 = auto()
    F5 = auto()
    F6 = auto()
    F7 = auto()
    F8 = auto()
    F9 = auto()
    F10 = auto()


def get_default_mi_attrs(some_enum: SomeEnum):
    if some_enum in (SomeEnum.A1, SomeEnum.A2, SomeEnum.A3):
        return "A"
    elif some_enum in (SomeEnum.B6,):
        return "B"
    elif some_enum in (SomeEnum.C1, SomeEnum.C2, SomeEnum.C3):
        return "C"
    elif some_enum in (SomeEnum.D4, SomeEnum.D5):
        return "D"
    elif some_enum in (SomeEnum.E9,):
        return "E"
    # uncommenting this part will cause ty to run slower in order of magnitude
    # elif some_enum in (SomeEnum.F1,):
    #     return "F"
```

```console
$ ty check -v tmp/midb.py 
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.9, platform: linux
INFO Indexed 1 file(s) in 0.003s
INFO Could not find class `enum.StrEnum` in typeshed on Python 3.9. Falling back to `Unknown` for the symbol instead.
INFO Checking file `/path/to/test.py` took more than 100ms (4.180301732s)
All checks passed!

# uncommenting the above
$ ty check -v tmp/midb.py 
INFO Defaulting to python-platform `linux`
INFO Python version: Python 3.9, platform: linux
INFO Indexed 1 file(s) in 0.003s
INFO Could not find class `enum.StrEnum` in typeshed on Python 3.9. Falling back to `Unknown` for the symbol instead.
INFO Checking file `/path/to/test.py` took more than 100ms (243.977428347s)
All checks passed!
```

---

_Removed from milestone `Stable` by @carljm on 2025-12-23 15:56_

---

_Added to milestone `M1` by @carljm on 2025-12-23 15:56_

---

_Label `enums` added by @AlexWaygood on 2026-01-03 21:24_

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:43_

---
