```yaml
number: 2432
title: "feature request: track \"exact\" types (excluding subclasses)"
type: issue
state: open
author: goodspark
labels:
  - wish
assignees: []
created_at: 2026-01-10T05:01:30Z
updated_at: 2026-01-13T04:00:30Z
url: https://github.com/astral-sh/ty/issues/2432
synced_at: 2026-01-13T04:30:28Z
```

# feature request: track "exact" types (excluding subclasses)

---

_@goodspark_

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

_Comment by @carljm on 2026-01-13 04:00_

Thanks for the report!

ty does not consider `Obj` to be an always-truthy type, because there could be subclasses of it which define `__bool__` to sometimes return `False`. Instances of such subclasses would not necessarily be truthy, and are part of the type `Obj`.

Of course in this case we could in theory tell that `a` cannot be an instance such a subclass, because we directly constructed an `Obj` instance just above. But making this distinction would require ty to track "exact" types (instance types that _don't_ include subclass instances). This would add a lot of complexity, and the usefulness of it is questionable, since it can only apply in limited scenarios, locally right after the construction of an instance from a concrete class.

Another workaround for this case would be to mark `Obj` as `@final` -- then ty will treat it as always truthy.

We can keep this open as a feature request for exact types, but I don't think that will be a priority in the near term.

---

_Label `wish` added by @carljm on 2026-01-13 04:00_

---

_Renamed from "Truthy checks in a boolean statement resolve to incorrect type" to "feature request: track "exact" types (excluding subclasses)" by @carljm on 2026-01-13 04:00_

---
