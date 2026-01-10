---
number: 21113
title: B008 triggers on immutable newtype when that type is imported from different file
type: issue
state: closed
author: tristanls-tbp
labels:
  - question
assignees: []
created_at: 2025-10-28T18:22:05Z
updated_at: 2025-10-28T21:11:46Z
url: https://github.com/astral-sh/ruff/issues/21113
synced_at: 2026-01-10T01:23:02Z
---

# B008 triggers on immutable newtype when that type is imported from different file

---

_Issue opened by @tristanls-tbp on 2025-10-28 18:22_

### Summary

## Works as intended use case
```python
# file1.py
from typing import NewType

MyStr = NewType("MyStr", str)

class MyClass:
    def __init__(self, my_str: MyStr = MyStr("immutable")):  # no issue
        self.my_str = my_str
```

## Unexpected use case
```python
# file1.py
from file2 import MyStr


class MyClass:
    def __init__(self, my_str: MyStr = MyStr("immutable")):  # B008 triggers here
        self.my_str = my_str
```
```python
# file2.py
from typing import NewType

MyStr = NewType("MyStr", str)
```

### Version

ruff 0.14.2 (83a3bc4ee 2025-10-23)

---

_Comment by @ntBre on 2025-10-28 18:58_

This is the expected behavior because Ruff doesn't currently support multi-file analysis. You can use the [extend-immutable-calls](https://docs.astral.sh/ruff/settings/#lint_flake8-bugbear_extend-immutable-calls) configuration option to mark `MyStr` as an immutable type.

---

_Label `question` added by @ntBre on 2025-10-28 18:58_

---

_Comment by @tristanls-tbp on 2025-10-28 21:11_

Ok, thank you.

---

_Closed by @tristanls-tbp on 2025-10-28 21:11_

---
