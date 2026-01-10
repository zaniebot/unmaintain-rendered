```yaml
number: 17181
title: "Regression: multiple assignments in match statement"
type: issue
state: closed
author: NeilGirdhar
labels:
  - bug
assignees: []
created_at: 2025-04-03T18:32:56Z
updated_at: 2025-04-03T21:32:40Z
url: https://github.com/astral-sh/ruff/issues/17181
synced_at: 2026-01-10T11:09:58Z
```

# Regression: multiple assignments in match statement

---

_Issue opened by @NeilGirdhar on 2025-04-03 18:32_

### Summary

```python
from dataclasses import dataclass


@dataclass
class ClassLabel:
    num_classes: int


def expectation_parameters(feature_type: ClassLabel | None) -> int:
    match feature_type:
        case ClassLabel(num_classes=num_classes):  # Multiple assignments here!?
            return num_classes
        case _:
            raise TypeError
```

### Version

0.11.3

---

_Assigned to @ntBre by @MichaReiser on 2025-04-03 18:37_

---

_Label `bug` added by @MichaReiser on 2025-04-03 18:37_

---

_Comment by @ntBre on 2025-04-03 19:30_

Thanks for the report! Should be an easy fix, I hope.

---

_Comment by @NeilGirdhar on 2025-04-03 19:34_

Thanks for looking into it!

---

_Closed by @ntBre on 2025-04-03 21:32_

---

_Closed by @ntBre on 2025-04-03 21:32_

---
