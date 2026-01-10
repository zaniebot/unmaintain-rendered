```yaml
number: 514
title: Intersection between Protocol and Final class is reduced incorrectly
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - set-theoretic types
  - Protocols
assignees: []
created_at: 2025-05-26T01:29:01Z
updated_at: 2025-10-01T16:16:29Z
url: https://github.com/astral-sh/ty/issues/514
synced_at: 2026-01-10T02:06:24Z
```

# Intersection between Protocol and Final class is reduced incorrectly

---

_Issue opened by @JelleZijlstra on 2025-05-26 01:29_

### Summary

Example:

```python
from typing import Callable, reveal_type, Literal, final, Protocol
from ty_extensions import Intersection

@final
class C:
    a: int | str

class P(Protocol):
    a: int

def f(
    x: Intersection[C, P],
):
    reveal_type(x)  # C, expect C & P
    reveal_type(x.a)  # int | str, expect int
```

https://play.ty.dev/8bb68376-9718-4602-8eea-694260da3335


### Version

_No response_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-26 06:33_

---

_Label `Protocols` added by @AlexWaygood on 2025-05-26 06:33_

---

_Label `bug` added by @AlexWaygood on 2025-05-26 06:37_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-05-26 06:37_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:32_

---

_Comment by @AlexWaygood on 2025-10-01 12:03_

This appears fixed on `main`. But we might want to add some more regression tests; I'll take a look.

Removing it from the beta milestone, anyway.

---

_Removed from milestone `Beta` by @AlexWaygood on 2025-10-01 12:03_

---

_Comment by @AlexWaygood on 2025-10-01 16:13_

It was fixed by https://github.com/astral-sh/ruff/pull/19043

---

_Comment by @AlexWaygood on 2025-10-01 16:16_

I think the regression tests added in that PR were sufficient to ensure that this won't regress again.

Thanks for the report!

---

_Closed by @AlexWaygood on 2025-10-01 16:16_

---
