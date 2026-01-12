```yaml
number: 2402
title: Stack overflow for recursive type
type: issue
state: open
author: chebee7i
labels:
  - fatal
assignees: []
created_at: 2026-01-08T18:13:33Z
updated_at: 2026-01-09T15:37:53Z
url: https://github.com/astral-sh/ty/issues/2402
synced_at: 2026-01-12T15:54:26Z
```

# Stack overflow for recursive type

---

_@chebee7i_

### Summary

Might be related to https://github.com/astral-sh/ty/issues/2360?

```python
from collections.abc import Mapping
from typing import TypeAlias

type JSONScalar = str | int | float | bool | None
type JSONLike = JSONScalar | tuple['JSONLike', ...] | Mapping[str, 'JSONLike']
JSONLikeAlt: TypeAlias = JSONScalar | tuple['JSONLikeAlt', ...] | Mapping[str, 'JSONLikeAlt']

# Comment this function out to remove stack overflow.
def _normalize_broken(x: object) -> JSONLike:
    # Intentionally crippled for bug report.
    if isinstance(x, (list, tuple)):
        return tuple(_normalize_broken(v) for v in x)
    return str(x)


def _normalize_works(x: object) -> JSONLikeAlt:
    # Intentionally crippled for bug report.
    if isinstance(x, (list, tuple)):
        return tuple(_normalize_works(v) for v in x)
    return str(x)
```

```
% pixi run ty --version
ty 0.0.10


% pixi run ty check example.py
thread '<unknown>' (3032748) has overflowed its stack
fatal runtime error: stack overflow, aborting
```
Using `TypeAlias` somehow avoids the issue.

### Version

ty 0.0.10

---

_Label `fatal` added by @AlexWaygood on 2026-01-08 23:43_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-09 00:08_

---

_Assigned to @carljm by @carljm on 2026-01-09 15:37_

---
