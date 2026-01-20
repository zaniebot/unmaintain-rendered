```yaml
number: 2553
title: "No `invalid-type-form` errors for invalid type forms in quoted annotations"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - typing semantics
assignees: []
created_at: 2026-01-18T12:22:29Z
updated_at: 2026-01-20T02:17:03Z
url: https://github.com/astral-sh/ty/issues/2553
synced_at: 2026-01-20T02:39:58Z
```

# No `invalid-type-form` errors for invalid type forms in quoted annotations

---

_@AlexWaygood_

### Summary

The typing conformance suite points out that we are supposed to emit an error here complaining about an invalid type form, but we do not; instead, we just silently infer `Unknown` for the `x` variable:

```py
x: "1234"
```

We do emit an error for the same expression if it is not quoted:

```py
x: 1234  # error [invalid-type-form] "Int literals are not allowed in this context in a type expression"
```

---

_Label `bug` added by @AlexWaygood on 2026-01-18 12:22_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-18 12:22_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-18 12:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-20 02:17_

---
