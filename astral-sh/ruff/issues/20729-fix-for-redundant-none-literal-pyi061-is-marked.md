```yaml
number: 20729
title: Fix for redundant-none-literal (PYI061) is marked as safe but should not be
type: issue
state: open
author: user27182
labels:
  - fixes
assignees: []
created_at: 2025-10-06T22:46:55Z
updated_at: 2025-10-07T00:20:50Z
url: https://github.com/astral-sh/ruff/issues/20729
synced_at: 2026-01-12T15:54:57Z
```

# Fix for redundant-none-literal (PYI061) is marked as safe but should not be

---

_@user27182_

### Summary

[PYI061](https://docs.astral.sh/ruff/rules/redundant-none-literal/) is marked as a safe fix but it's not, and breaks code that uses `typing.get_args` to do input validation based on type hints. E.g. the two outputs from `get_args` are _not_ the same before and after the fix
``` py
>>> from typing import Literal, get_args
>>> options = Literal['foo', 'bar', None]  # violates PYI061
>>> print(get_args(options))
('foo', 'bar', None)

>>> options = Literal['foo', 'bar'] | None  # auto-fix for PYI061
>>> print(get_args(options))
(typing.Literal['foo', 'bar'], <class 'NoneType'>)
```

This auto-fix breaks this code:
``` py
def thing(arg: options):
    # Validate input
    assert arg in get_args(options)
    # Do useful stuff
    ...
```

### Version

v0.13.3 (from pre-commit mirror)

---

_Label `fixes` added by @amyreese on 2025-10-07 00:20_

---
