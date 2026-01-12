```yaml
number: 912
title: "PyQt/PySide `unsupported-operator` error"
type: issue
state: closed
author: my1e5
labels:
  - question
assignees: []
created_at: 2025-07-29T13:55:25Z
updated_at: 2025-07-29T15:43:16Z
url: https://github.com/astral-sh/ty/issues/912
synced_at: 2026-01-12T15:54:24Z
```

# PyQt/PySide `unsupported-operator` error

---

_@my1e5_

### Summary

I searched the issue tracker for `unsupported-operator` but couldn't determine exactly if this was a duplicate issue.

## MRE
```py
from PySide6.QtCore import Qt
from PySide6.QtGui import QKeySequence

QKeySequence(Qt.Modifier.CTRL | Qt.Key.Key_P)
```
Since ty `0.0.1a15` this is triggering an error:

### `0.0.1a15`
```
$ uvx --with pyside6 --with ty==0.0.1a15 ty check dev/ty.py
Installed 5 packages in 3.46s
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[unsupported-operator]: Operator `|` is unsupported between objects of type `Modifier` and `Key`
 --> dev\ty.py:4:14
  |
2 | from PySide6.QtGui import QKeySequence
3 |
4 | QKeySequence(Qt.Modifier.CTRL | Qt.Key.Key_P)
  |              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `unsupported-operator` is enabled by default

Found 1 diagnostic
```

### `0.0.1a14`
```
$ uvx --with pyside6 --with ty==0.0.1a14 ty check dev/ty.py
Installed 5 packages in 3.51s
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
All checks passed!
```

Reference  - https://doc.qt.io/qtforpython-6/PySide6/QtGui/QKeySequence.html#detailed-description

### Version

ty 0.0.1-alpha.16 (c452e53a0 2025-07-25)

---

_Label `bug` added by @sharkdp on 2025-07-29 14:17_

---

_Assigned to @sharkdp by @sharkdp on 2025-07-29 14:17_

---

_Comment by @sharkdp on 2025-07-29 14:18_

Thank you for reporting this.

We now support enums, but we don't properly support `enum.Flag`-based classes yet (#876).

---

_Comment by @sharkdp on 2025-07-29 14:48_

Upon closer inspection (and realizing that this OR-s together two different enums), I'm actually not sure this is a bug. The stubs for `QtCore` seem incomplete. This is `Modifier`:
```pyi
class Modifier(enum.Flag):
    SHIFT                     = ...  # 0x2000000
    CTRL                      = ...  # 0x4000000
    ALT                       = ...  # 0x8000000
    META                      = ...  # 0x10000000
    MODIFIER_MASK             = ...  # 0xfe000000
```

And `Key` looks like:
```pyi
class Key(enum.IntEnum):
    Key_Any                   = ...  # 0x20
    # […]
    Key_unknown               = ...  # 0x1ffffff
```

Notably, there is no custom `__or__` method that would allow `Modifier` and `Key` to be OR-ed. And `enum.Flag`'s `__or__` method is not applicable, because it requires `other` to be of the same type:

```pyi
class Flag(Enum):
    # […]
    def __or__(self, other: Self) -> Self: ...
```


pyright also issues an error:
```
main.py:5:14 - error: Operator "|" not supported for types "Literal[Modifier.CTRL]" and "Literal[Key.Key_P]" (reportOperatorIssue)
```

---

_Label `bug` removed by @sharkdp on 2025-07-29 14:49_

---

_Label `question` added by @sharkdp on 2025-07-29 14:49_

---

_Closed by @sharkdp on 2025-07-29 15:43_

---
