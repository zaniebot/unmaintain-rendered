```yaml
number: 9890
title: "False positive `SLOT000` on Enum subclasses"
type: issue
state: closed
author: jamesbraza
labels:
  - rule
assignees: []
created_at: 2024-02-08T06:12:35Z
updated_at: 2024-04-18T01:15:53Z
url: https://github.com/astral-sh/ruff/issues/9890
synced_at: 2026-01-12T15:54:49Z
```

# False positive `SLOT000` on Enum subclasses

---

_@jamesbraza_

With `ruff==0.2.1` and Python 3.12:

```python
from enum import Enum

class MyEnum(Enum):
    pass

class CustomStrEnum(str, MyEnum):  # Throws SLOT000
    pass
```

I think this is a false positive for [`SLOT000`](https://docs.astral.sh/ruff/rules/no-slots-in-str-subclass/) based on https://github.com/astral-sh/ruff/issues/5748. At the time, looks like Ruff decided not to check for this.

However, maybe nowadays it's possible, and it is a false positive `SLOT000`. Do you think we can fix this false positive

---

_Label `rule` added by @AlexWaygood on 2024-02-08 09:36_

---

_Comment by @charliermarsh on 2024-04-18 00:52_

We can fix this for references in the same file.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-18 00:52_

---

_Closed by @charliermarsh on 2024-04-18 01:15_

---
