```yaml
number: 19386
title: "strEnum TypeError with non-string values doesn't raise an error or warning"
type: issue
state: open
author: Tomas542
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-16T16:37:42Z
updated_at: 2025-07-16T16:59:10Z
url: https://github.com/astral-sh/ruff/issues/19386
synced_at: 2026-01-12T15:54:56Z
```

# strEnum TypeError with non-string values doesn't raise an error or warning

---

_@Tomas542_

### Summary

I'm not sure if this is within the scope of `ruff`, so apologies if it isn't. The `strEnum` class is used to define enums with string attributes. So I think `ruff` should give a warning/error when we define a non-string attribute (which will cause a `TypeError` when the script runs). Both `ruff` and `pylance` (in the list with default rules) do nothing.

```python
from enum import StrEnum


class Foo(StrEnum):
    A = 1
    B = "aaa"


print(Foo("aaa"))
```

### Version

_No response_

---

_Label `rule` added by @ntBre on 2025-07-16 16:59_

---

_Label `needs-decision` added by @ntBre on 2025-07-16 16:59_

---
