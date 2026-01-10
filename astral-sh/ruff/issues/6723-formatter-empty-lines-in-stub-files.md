```yaml
number: 6723
title: "Formatter: Empty lines in stub files"
type: issue
state: closed
author: konstin
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-21T09:57:24Z
updated_at: 2023-09-11T08:04:02Z
url: https://github.com/astral-sh/ruff/issues/6723
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: Empty lines in stub files

---

_Issue opened by @konstin on 2023-08-21 09:57_

When formatting typeshed (`cargo run --bin ruff_dev -- format-dev --write typeshed`), there is still a big diff with black wrt to empty lines.

E.g. given
```python
class Feature:
    def getMandatoryRelease(self) -> None: ...
    compiler_flag: int

def getMandatoryRelease(self) -> None: ...
compiler_flag: int

if sys.platform != "win32":
    def can_change_color() -> bool: ...
    # Changed in Python 3.8.8 and 3.9.2
    if sys.version_info >= (3, 8):
        pass

if sys.platform == "win32":
    class Dummy:
        __new__: None
        __init__: None

    # Actual typename SummaryInformation, not exposed by the implementation
    class _SummaryInformation:
        def GetProperty(self, field: int) -> int | bytes | None: ...
```
black will format this as
```python
class Feature:
    def getMandatoryRelease(self) -> None: ...
    compiler_flag: int

def getMandatoryRelease(self) -> None: ...

compiler_flag: int

if sys.platform != "win32":
    def can_change_color() -> bool: ...
    # Changed in Python 3.8.8 and 3.9.2
    if sys.version_info >= (3, 8):
        pass

if sys.platform == "win32":
    class Dummy:
        __new__: None
        __init__: None

    # Actual typename SummaryInformation, not exposed by the implementation
    class _SummaryInformation:
        def GetProperty(self, field: int) -> int | bytes | None: ...
```
while we format this as
```python
class Feature:
    def getMandatoryRelease(self) -> None: ...

    compiler_flag: int

def getMandatoryRelease(self) -> None: ...

compiler_flag: int

if sys.platform != "win32":

    def can_change_color() -> bool: ...

    # Changed in Python 3.8.8 and 3.9.2
    if sys.version_info >= (3, 8):
        pass

if sys.platform == "win32":

    class Dummy:
        __new__: None
        __init__: None

    # Actual typename SummaryInformation, not exposed by the implementation
    class _SummaryInformation:
        def GetProperty(self, field: int) -> int | bytes | None: ...
```

The task is to go through the typeshed diff and figure out each empty line rule we're still missing. If you implement this you don't need to (and probably shouldn't) fix everything at once but you can implement any part of it (as long as it doesn't decrease the similarity index).

---

_Label `formatter` added by @konstin on 2023-08-21 09:57_

---

_Label `help wanted` added by @konstin on 2023-08-21 09:57_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:14_

---

_Assigned to @konstin by @konstin on 2023-09-06 17:10_

---

_Closed by @konstin on 2023-09-11 08:04_

---
