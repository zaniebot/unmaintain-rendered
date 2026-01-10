```yaml
number: 1559
title: "Emit a diagnostic if a `@type_check_only` symbol is imported in a runtime context"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-11-14T17:07:55Z
updated_at: 2025-11-18T16:10:41Z
url: https://github.com/astral-sh/ty/issues/1559
synced_at: 2026-01-10T01:58:59Z
```

# Emit a diagnostic if a `@type_check_only` symbol is imported in a runtime context

---

_Issue opened by @AlexWaygood on 2025-11-14 17:07_

E.g.

````md

`stub.pyi`:

```pyi
from typing import type_check_only, Protocol

@type_check_only
class SupportsNext(Protocol):
    def __next__(self): ...

X: SupportsNext
```

`main.py`:

```py
# We should emit a diagnostic here:
# `SupportsNext` doesn't exist at runtime, it's only for the type checker
from stub import SupportsNext
```
````

The diagnostic should not fire if the `import` statement is inside a stub file. Ideally we would also suppress the diagnostic if the `import` statement is inside an `if TYPE_CHECKING` block, but doing that well depends on implementing https://github.com/astral-sh/ty/issues/1553.

---

_Label `typing semantics` added by @AlexWaygood on 2025-11-14 17:07_

---

_Comment by @carljm on 2025-11-14 17:15_

It looks to me like neither pyright (not even in strict mode) nor mypy offer this diagnostic? Marking it as post-stable for that reason.

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 17:15_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
