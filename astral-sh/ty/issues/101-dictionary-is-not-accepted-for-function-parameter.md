```yaml
number: 101
title: "Dictionary is not accepted for function parameter annotated with `Mapping`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-05-03T14:32:45Z
updated_at: 2025-05-07T19:21:12Z
url: https://github.com/astral-sh/ty/issues/101
synced_at: 2026-01-10T02:34:09Z
```

# Dictionary is not accepted for function parameter annotated with `Mapping`

---

_Issue opened by @charliermarsh on 2025-05-03 14:32_

```python
from typing import Mapping

def func(headers: Mapping[str, str] | None = None):
    ...

func(headers={"foo": "bar"})
```

See: https://types.ruff.rs/c06d5861-9b0a-49e4-8e76-22adf0c294d6


---

_Assigned to @dcreager by @AlexWaygood on 2025-05-03 14:38_

---

_Label `bug` added by @AlexWaygood on 2025-05-03 14:38_

---

_Comment by @AlexWaygood on 2025-05-03 14:38_

Not sure if this is a known issue or not -- @dcreager?

---

_Closed by @dcreager on 2025-05-07 19:21_

---
