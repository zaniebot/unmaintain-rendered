```yaml
number: 567
title: "Attributes of `types.FunctionType` should be available on `lambda` functions"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - good first issue
  - help wanted
  - runtime semantics
assignees: []
created_at: 2025-06-02T11:20:26Z
updated_at: 2025-06-02T15:46:28Z
url: https://github.com/astral-sh/ty/issues/567
synced_at: 2026-01-10T02:34:10Z
```

# Attributes of `types.FunctionType` should be available on `lambda` functions

---

_Issue opened by @AlexWaygood on 2025-06-02 11:20_

### Summary

ty currently emits a false-positive diagnostic on this snippet:

```py
from typing import reveal_type

x = lambda y: y

# error[unresolved-attribute] Type `(y) -> Unknown` has no attribute `__code__`
reveal_type(x.__code__)  # revealed: Unknown
```

https://play.ty.dev/84a1b067-9670-4463-82ce-0fc025997c91

All `lambda` functions are instances of `types.FunctionType`, so all attributes available on `types.FunctionType` instances should also be available on `lambda`s.

Following https://github.com/astral-sh/ruff/pull/18242 this is easy to fix: we just need to change this line so that it calls `CallableType::function_like()` rather than `CallableType::single()` (and add some tests).

---

_Label `bug` added by @AlexWaygood on 2025-06-02 11:20_

---

_Label `help wanted` added by @AlexWaygood on 2025-06-02 11:20_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-06-02 11:20_

---

_Label `good first issue` added by @MichaReiser on 2025-06-02 11:24_

---

_Comment by @lipefree on 2025-06-02 11:59_

I can try to resolve this issue !

---

_Closed by @AlexWaygood on 2025-06-02 15:46_

---
