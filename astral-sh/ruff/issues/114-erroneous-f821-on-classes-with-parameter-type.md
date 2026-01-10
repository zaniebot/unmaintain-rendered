```yaml
number: 114
title: Erroneous F821 on classes with parameter type annotations
type: issue
state: closed
author: cjfuller
labels: []
assignees: []
created_at: 2022-09-06T22:36:46Z
updated_at: 2022-09-07T00:53:52Z
url: https://github.com/astral-sh/ruff/issues/114
synced_at: 2026-01-10T15:56:05Z
```

# Erroneous F821 on classes with parameter type annotations

---

_Issue opened by @cjfuller on 2022-09-06 22:36_

If I run ruff on a file containing a class with a type annotation at class level, it incorrectly gives F821 (undefined name).

Examples:

```python

from dataclasses import dataclass


@dataclass
class Bad:
    x: int


@dataclass
class AlsoBad:
    x: int = 3


class StillBad:
    x: int

    def __init__(self):
        self.x = 3


class Ok:
    x = 3
```

Running `ruff <filename>` on this file gives:
```
ruff_test.py:6:5: F821 Undefined name `x`
ruff_test.py:11:5: F821 Undefined name `x`
ruff_test.py:15:5: F821 Undefined name `x`
```
(flake8 gives no errors)


Ruff version 0.0.28, python 3.8.13, ubuntu 22.04.1.

---

_Comment by @cjfuller on 2022-09-06 22:51_

Actually I guess that this also happens with top-level definitions too:
```python
y: int = 4
```
also gives 
```
ruff_test.py:25:1: F821 Undefined name `y`
```

---

_Comment by @charliermarsh on 2022-09-07 00:21_

I think I have to fix this in RustPython.

---

_Closed by @charliermarsh on 2022-09-07 00:53_

---
