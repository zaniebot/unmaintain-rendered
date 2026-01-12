```yaml
number: 2450
title: list of randints is not list of ints...
type: issue
state: closed
author: danielpopescu
labels: []
assignees: []
created_at: 2026-01-11T20:28:29Z
updated_at: 2026-01-11T20:30:55Z
url: https://github.com/astral-sh/ty/issues/2450
synced_at: 2026-01-12T02:26:12Z
```

# list of randints is not list of ints...

---

_Issue opened by @danielpopescu on 2026-01-11 20:28_

### Summary

```
import random

x = [random.randint(0, 100) for _ in range(10)]

```

x has type of list[int | Unknown] as opposed to just list[int] 

### Version

_No response_

---

_Closed by @AlexWaygood on 2026-01-11 20:30_

---

_Comment by @AlexWaygood on 2026-01-11 20:30_

Thanks! This is intentional behaviour but we intend to change it soon — see #1240 for more details 

---
