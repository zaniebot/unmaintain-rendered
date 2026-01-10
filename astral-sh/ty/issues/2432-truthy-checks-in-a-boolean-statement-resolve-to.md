```yaml
number: 2432
title: Truthy checks in a boolean statement resolve to incorrect type
type: issue
state: open
author: goodspark
labels: []
assignees: []
created_at: 2026-01-10T05:01:30Z
updated_at: 2026-01-10T07:31:36Z
url: https://github.com/astral-sh/ty/issues/2432
synced_at: 2026-01-10T11:36:43Z
```

# Truthy checks in a boolean statement resolve to incorrect type

---

_Issue opened by @goodspark on 2026-01-10 05:01_

### Summary

Playground: https://play.ty.dev/fddded41-5c96-4852-9849-bd44cdf03074

```py
from datetime import datetime, timedelta
from dataclasses import dataclass

@dataclass
class Obj:
    x: datetime

a: Obj | None = Obj(datetime.now())
# Since a is always truthy, the boolean statement continues and returns datetime
b = (a and a.x - timedelta(days=1))
# c should always be datetime here, so the diagnostic is incorrect
c = datetime.now() + timedelta(days=1) > b
# Operator `>` is not supported between objects of type `datetime` and `(Obj & ~AlwaysTruthy) | datetime`
```

If `a` was just `datetime | None`, this would work fine, but this use case is meaningful as often container data types might be None (ex. a DB row lookup).

To be fair, this diagnostic is useful _if `a` was not AlwaysTruthy, such as through a function call (which is more realistic)_. However, the diagnostic message is misleading as it currently is. One could argue instead we should write code using an if or ternary expression.

```py
b = None
if a:
    b = a.x - timedelta(days=1)
# or ...
b = None if a is None else a.x - timedelta(days=1)
```

With this change, ty will correctly infer `b` is always datetime and have no issue. And it will also correctly error with an accurate message if `a` was not AlwaysTruthy (ie. None can't be compared to datetime).

---

Terms used to find already-reported issues:
- `unsupported-operation boolean`
- `unsupported-operation and`
- `unsupported-operation truthy`

#1972 might be a match. It talks about types being constrained by values. In a way, the type of `b` does get constrained by the resulting value of evaluating the compound boolean expression. But my example is not using a direct value but instead the resulting value of an operation.

### Version

0.0.11

---
