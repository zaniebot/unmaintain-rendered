```yaml
number: 2309
title: "`inconsistent-mro`: add autofix that moves `Generic[]` to the end of the bases list, if doing so would fix the violation"
type: issue
state: open
author: AlexWaygood
labels:
  - fixes
assignees: []
created_at: 2026-01-02T20:55:08Z
updated_at: 2026-01-02T20:55:08Z
url: https://github.com/astral-sh/ty/issues/2309
synced_at: 2026-01-10T01:56:41Z
```

# `inconsistent-mro`: add autofix that moves `Generic[]` to the end of the bases list, if doing so would fix the violation

---

_Issue opened by @AlexWaygood on 2026-01-02 20:55_

The first of these causes ty to issue an `inconsistent-mro` violation; the second one does not:

```py
from typing import Generic, TypeVar

K = TypeVar("K")
V = TypeVar("V")

class Foo(Generic[K, V], dict): ...
class Bar(dict, Generic[K, V]): ...
```

And issues involving `Generic[]` are probably some of the most common causes of `inconsistent-mro` violations in user code. We should offer an autofix that moves the `Generic[]` element to the end of the bases list, if doing so would fix the `inconsistent-mro` violation.

---

_Label `fixes` added by @AlexWaygood on 2026-01-02 20:55_

---
