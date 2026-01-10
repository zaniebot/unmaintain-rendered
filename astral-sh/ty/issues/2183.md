```yaml
number: 2183
title: Checking large Enum takes very long time
type: issue
state: closed
author: ido123net
labels:
  - performance
  - enums
assignees: []
created_at: 2025-12-23T10:26:30Z
updated_at: 2025-12-23T11:40:39Z
url: https://github.com/astral-sh/ty/issues/2183
synced_at: 2026-01-10T01:56:41Z
```

# Checking large Enum takes very long time

---

_Issue opened by @ido123net on 2025-12-23 10:26_

### Summary

When trying to check code containing large Enum inside can cause `ty` to run very slow.

steps to reproduce:
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

### Version

ty 0.0.5

---

_Label `performance` added by @AlexWaygood on 2025-12-23 10:32_

---

_Label `enums` added by @AlexWaygood on 2025-12-23 10:32_

---

_Renamed from "Parsing large Enum takes very long time" to "Checking large Enum takes very long time" by @MichaReiser on 2025-12-23 10:37_

---

_Comment by @MichaReiser on 2025-12-23 10:38_

Thank you. I suspect that this is the same as https://github.com/astral-sh/ty/issues/1588

---

_Closed by @MichaReiser on 2025-12-23 10:38_

---

_Comment by @ido123net on 2025-12-23 11:40_

@MichaReiser thx, you are right, I searched for the `StrEnum` in the open issues (my bad).

In any case, I think my reproduction is simpler :wink:

---
