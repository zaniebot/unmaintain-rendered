```yaml
number: 1831
title: "Assignability of `type[T]` to union of callables"
type: issue
state: closed
author: carljm
labels: []
assignees: []
created_at: 2025-12-10T00:36:24Z
updated_at: 2025-12-10T04:48:09Z
url: https://github.com/astral-sh/ty/issues/1831
synced_at: 2026-01-12T15:54:25Z
```

# Assignability of `type[T]` to union of callables

---

_@carljm_

```py
from typing import Callable, Any

def needs_a_callable(x: Callable[[Any], Any] | Callable[[Any, object], Any]): ...

class F:
    def __init__(self, arg) -> None: ...

def f[T: F](x: T):
    needs_a_callable(type(x))
```

It looks like we might not be implementing assignability correctly for `type[T]` vs a _union_ of callables?

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/issues/21685#issuecomment-3591695954_
            

---

_Added to milestone `Stable` by @carljm on 2025-12-10 00:37_

---

_Comment by @ibraheemdev on 2025-12-10 03:33_

Hmm.. this should have been fixed by https://github.com/astral-sh/ruff/pull/21740?

---

_Comment by @carljm on 2025-12-10 03:42_

Oh feel free to close then -- I was just going off the discussion in the other PR, I don't think I even tested (I should have).

---

_Comment by @ibraheemdev on 2025-12-10 04:48_

Yeah, this works fine now.

---

_Closed by @ibraheemdev on 2025-12-10 04:48_

---
