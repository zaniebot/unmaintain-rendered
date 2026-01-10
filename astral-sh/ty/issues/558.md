```yaml
number: 558
title: failing to infer the right type for an attributes  with optional type in an if else statement
type: issue
state: closed
author: martinResearch
labels: []
assignees: []
created_at: 2025-05-31T16:12:31Z
updated_at: 2025-05-31T16:19:49Z
url: https://github.com/astral-sh/ty/issues/558
synced_at: 2026-01-10T02:34:10Z
```

# failing to infer the right type for an attributes  with optional type in an if else statement

---

_Issue opened by @martinResearch on 2025-05-31 16:12_

### Summary

The following code gives an error, is it expected ? mypy is not providing any error:

```
from dataclasses import dataclass


@dataclass
class Coordinate:
    x: int | None
    y: int | None


def f(coord: Coordinate) -> int:
    if coord.x is None:
        out = 5
    else:
        out = coord.x
    return out

```
ty gives the error

```     
  return out
         ^^^ expected `int`, found `int | None``
```

using an intermediate variable fixes the problem:
```
def f(coord: Coordinate) -> int:
    x = coord.x
    if x is None:
        out = 5
    else:
        out = x
    return out
```

### Version

ty 0.0.1a7

---

_Closed by @AlexWaygood on 2025-05-31 16:19_

---
