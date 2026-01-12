```yaml
number: 996
title: "Negative narrowing with `type(...) is` and final types"
type: issue
state: closed
author: JelleZijlstra
labels:
  - narrowing
assignees: []
created_at: 2025-08-15T05:07:33Z
updated_at: 2025-08-15T13:52:31Z
url: https://github.com/astral-sh/ty/issues/996
synced_at: 2026-01-12T15:54:24Z
```

# Negative narrowing with `type(...) is` and final types

---

_@JelleZijlstra_

### Summary

When doing type narrowing with `type(...) is`, ty doesn't narrow in the negative case, which is generally correct because the value could be a subclass of the given type. However, if the type is `@final`, it's safe to narrow in the negative case too:

```python
def f(x: bool | str):
    if type(x) is bool:
        reveal_type(x)  # bool (correct)
    else:
        reveal_type(x)  # bool | str (expect str)
```

https://play.ty.dev/b32ed746-fadf-482e-86f3-4af12e0bc622

### Version

_No response_

---

_Label `narrowing` added by @AlexWaygood on 2025-08-15 07:45_

---

_Closed by @AlexWaygood on 2025-08-15 13:52_

---
