```yaml
number: 17911
title: "[ty] `NoReturn`/`Never` are not handled correctly"
type: issue
state: closed
author: Bobronium
labels: []
assignees: []
created_at: 2025-05-07T12:39:53Z
updated_at: 2025-05-07T12:47:24Z
url: https://github.com/astral-sh/ruff/issues/17911
synced_at: 2026-01-10T11:09:58Z
```

# [ty] `NoReturn`/`Never` are not handled correctly

---

_Issue opened by @Bobronium on 2025-05-07 12:39_

### Summary

`cat test.py`
```python
import os
from typing import Never, reveal_type


def fast_exit(code: int = 0) -> Never:
    os._exit(code)



try:
    foo = "bar"
except KeyboardInterrupt:
    fast_exit()

print(foo)
reveal_type(fast_exit())
```
`ty check test.py --output-format concise`
```python
error[lint:invalid-return-type] test.py:5:33: Function can implicitly return `None`, which is not assignable to return type `Never`
warning[lint:possibly-unresolved-reference] test.py:15:7: Name `foo` used when possibly not defined
info[revealed-type] test.py:16:1: Revealed type: `Never`
Found 3 diagnostics
```

Adding `raise` statement after `fast_exit()` solves `possibly-unresolved-reference` as well as adding `raise` to `fast_exit` solves `invalid-return-type`:

```python
def fast_exit(code: int = 0) -> Never:
    os._exit(code)
    raise
```

```python
try:
    foo = "bar"
except KeyboardInterrupt:
    fast_exit()
    raise
```

### Version

ty 0.0.0-alpha.5 (ff9000864 2025-05-06)

---

_Closed by @Bobronium on 2025-05-07 12:47_

---
