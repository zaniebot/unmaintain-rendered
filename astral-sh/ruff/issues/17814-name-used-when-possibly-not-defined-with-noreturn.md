```yaml
number: 17814
title: "Name `...` used when possibly not defined with `NoReturn` branch"
type: issue
state: closed
author: charliermarsh
labels:
  - ty
assignees: []
created_at: 2025-05-03T14:23:55Z
updated_at: 2025-05-03T14:26:02Z
url: https://github.com/astral-sh/ruff/issues/17814
synced_at: 2026-01-12T15:54:56Z
```

# Name `...` used when possibly not defined with `NoReturn` branch

---

_@charliermarsh_

`value` here is always defined because `exit` is `NoReturn`, but `ty` reports it as possibly not defined.

```python
import random


if random.random() > 0.5:
    exit(0)
else:
    value = 1

print(value)
```

See: https://types.ruff.rs/eef5be97-af38-4588-9c25-4fd69565baa3

---

_Label `red-knot` added by @charliermarsh on 2025-05-03 14:23_

---

_Closed by @AlexWaygood on 2025-05-03 14:26_

---
