```yaml
number: 1651
title: Emit diagnostic when a type variable with a default is followed by one without a default
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-11-27T03:45:12Z
updated_at: 2025-12-14T19:35:38Z
url: https://github.com/astral-sh/ty/issues/1651
synced_at: 2026-01-10T01:55:00Z
```

# Emit diagnostic when a type variable with a default is followed by one without a default

---

_Issue opened by @dhruvmanila on 2025-11-27 03:45_

```python
from typing import TypeVar, Generic

T1 = TypeVar("T1", default=int)
T2 = TypeVar("T2")

# TypeError: Type parameter ~T2 without a default follows type parameter with a default
class Foo1(Generic[T1, T2]): ...

# SyntaxError: non-default type parameter 'T4' follows default type parameter
class Foo2[T3 = int, T4]: ...
```

ty should just focus on the `TypeError` while the `SyntaxError` would need to be raised by the parser (https://github.com/astral-sh/ruff/issues/17412#issuecomment-3584088217) 

---

_Label `help wanted` added by @dhruvmanila on 2025-11-27 03:45_

---

_Label `diagnostics` added by @dhruvmanila on 2025-11-27 03:45_

---

_Added to milestone `Stable` by @dhruvmanila on 2025-11-27 03:45_

---

_Comment by @11happy on 2025-12-04 10:17_

I have linked the PR : ) 
Thank you

---

_Renamed from "Raise diagnostic when a type variable with a default is followed by one without a default" to "Emit diagnostic when a type variable with a default is followed by one without a default" by @AlexWaygood on 2025-12-04 11:11_

---

_Closed by @AlexWaygood on 2025-12-14 19:35_

---
