```yaml
number: 986
title: "ty incorrectly thinks all objects have an `__mro__` attribute"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - runtime semantics
  - attribute access
assignees: []
created_at: 2025-08-14T14:59:52Z
updated_at: 2025-10-27T11:19:14Z
url: https://github.com/astral-sh/ty/issues/986
synced_at: 2026-01-10T02:06:24Z
```

# ty incorrectly thinks all objects have an `__mro__` attribute

---

_Issue opened by @AlexWaygood on 2025-08-14 14:59_

### Summary

```py
reveal_type(print.__mro__)  # revealed: tuple[<class 'FunctionType'>, <class 'object'>]
reveal_type(object().__mro__)  # revealed: tuple[<class 'object'>]
```

^Both those attribute accesses fail at runtime, but ty doesn't emit any diagnostics here.

https://play.ty.dev/ffdf65ae-e05f-493e-9fec-5ea6675786c9

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-08-14 14:59_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-08-14 14:59_

---

_Label `attribute access` added by @AlexWaygood on 2025-08-14 22:05_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-22 13:11_

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-10-15 18:45_

---

_Closed by @AlexWaygood on 2025-10-27 11:19_

---
