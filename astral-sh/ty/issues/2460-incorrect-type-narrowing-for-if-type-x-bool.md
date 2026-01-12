```yaml
number: 2460
title: "Incorrect type narrowing for `if type(x) == bool`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - narrowing
assignees: []
created_at: 2026-01-12T11:30:20Z
updated_at: 2026-01-12T12:26:49Z
url: https://github.com/astral-sh/ty/issues/2460
synced_at: 2026-01-12T15:54:26Z
```

# Incorrect type narrowing for `if type(x) == bool`

---

_@AlexWaygood_

### Summary

```py
def f(x: object):
    if type(x) == bool:
        reveal_type(x)  # revealed: ~bool
    else:
        reveal_type(x)  # revealed: ~bool
```

We shouldn't be doing any narrowing at all here, let alone narrowing the type to `~bool`. We should narrow for constructs like `if type(x) is bool` or `if type(x) is not bool`, but it's unsafe to apply this kind of narrowing for other operators like `==` or `!=` (the type of `x` might have a custom metaclass that allows it to compare equal to `bool` even though it is not `bool`)

### Version

_No response_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-12 11:30_

---

_Label `bug` added by @AlexWaygood on 2026-01-12 11:30_

---

_Label `narrowing` added by @AlexWaygood on 2026-01-12 11:30_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-12 12:16_

---

_Closed by @AlexWaygood on 2026-01-12 12:26_

---
