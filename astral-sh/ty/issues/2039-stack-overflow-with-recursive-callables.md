```yaml
number: 2039
title: Stack overflow with recursive callables
type: issue
state: open
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-17T21:09:55Z
updated_at: 2026-01-08T17:49:01Z
url: https://github.com/astral-sh/ty/issues/2039
synced_at: 2026-01-12T15:54:26Z
```

# Stack overflow with recursive callables

---

_@correctmost_

### Summary

ty crashes when checking this code:

```python
from typing import Callable

def fn[**T, U](c: Callable[T, U]) -> Callable[T, U]:
    return c

fn(fn)
```

```
thread '<unknown>' (2580764) has overflowed its stack
fatal runtime error: stack overflow, aborting
```

### Version

a15bc9c249 w/ astral-sh/ruff@b36ff75a2

---

_Comment by @carljm on 2025-12-17 21:14_

Ha, nice. Thanks for the report!

---

_Label `fatal` added by @carljm on 2025-12-17 21:14_

---

_Added to milestone `Stable` by @carljm on 2025-12-17 21:14_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-17 21:16_

---

_Label `bug` added by @AlexWaygood on 2025-12-17 21:16_

---

_Assigned to @dcreager by @dcreager on 2025-12-18 00:37_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:39_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:39_

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-08 17:49_

---

_Added to milestone `Stable` by @carljm on 2026-01-08 17:49_

---
