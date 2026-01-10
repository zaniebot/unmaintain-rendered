```yaml
number: 1873
title: "`__qualname__` not recognized inside class body"
type: issue
state: closed
author: randolf-scholz
labels:
  - runtime semantics
assignees: []
created_at: 2025-12-13T08:07:59Z
updated_at: 2025-12-13T22:10:26Z
url: https://github.com/astral-sh/ty/issues/1873
synced_at: 2026-01-10T01:55:00Z
```

# `__qualname__` not recognized inside class body

---

_Issue opened by @randolf-scholz on 2025-12-13 08:07_

### Summary

`ty` does not know that about certain [special attributes](https://docs.python.org/3/reference/datamodel.html#special-attributes) are already available when creating the class (namely `__qualname__`, `__module__` and `__firstlineno__`) https://play.ty.dev/5b18389b-f35c-4f0c-9d53-f9c8bac4526e

```python
from typing import reveal_type
class Foo:
    print(vars())
    reveal_type(__qualname__)      # E: unresolved-reference
    reveal_type(__module__)        # E: unresolved-reference
    reveal_type(__firstlineno__)   # E: unresolved-reference
```



Real world example: https://play.ty.dev/1ad0e7a0-7850-4445-98be-a92dc69e3cec

```python
import logging
from typing import ClassVar

class WithLogger:
    logger: ClassVar[logging.Logger] = logging.getLogger(__qualname__)
```

### Version

_No response_

---

_Label `runtime semantics` added by @sharkdp on 2025-12-13 08:56_

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-13 12:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-13 19:33_

---

_Closed by @charliermarsh on 2025-12-13 22:10_

---
