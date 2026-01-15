```yaml
number: 2509
title: "Annotating an instance attribute with `Final` means no type is shown when hovering over it"
type: issue
state: open
author: qmg-tmay
labels:
  - server
assignees: []
created_at: 2026-01-15T10:14:26Z
updated_at: 2026-01-15T15:06:29Z
url: https://github.com/astral-sh/ty/issues/2509
synced_at: 2026-01-15T15:50:04Z
```

# Annotating an instance attribute with `Final` means no type is shown when hovering over it

---

_@qmg-tmay_

### Summary

When a field is set without a explicit type hint, hovering over the instance member correctly infers the type (inherited from the method arguments):

<img width="308" height="99" alt="Image" src="https://github.com/user-attachments/assets/37e63031-1f66-4059-80be-58bcc4f99487" />

However, if the `typing.Final` type hint is used, hovering now reveals the type to be `Unknown`:

<img width="308" height="99" alt="Image" src="https://github.com/user-attachments/assets/c05e7a28-2e15-410e-ab09-ef274cacb5b1" />

Subsequent usages of the instance member _do_ correctly infer the type as `str`

### Version

ty 0.0.12 (Homebrew 2026-01-14)

---

_Comment by @AlexWaygood on 2026-01-15 10:28_

Similar to #1594

---

_Label `server` added by @AlexWaygood on 2026-01-15 10:28_

---

_Renamed from "Use of `Final` in class `__init__` erases instance member type information" to "Annotating an instance attribute with `Final` means no type is shown when hovering over it" by @AlexWaygood on 2026-01-15 15:06_

---
