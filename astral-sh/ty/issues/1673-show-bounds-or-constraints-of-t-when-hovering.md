```yaml
number: 1673
title: "Show bounds or constraints of `T` when hovering over a variable that has type `T`"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - generics
assignees: []
created_at: 2025-11-29T15:21:30Z
updated_at: 2025-11-29T15:21:30Z
url: https://github.com/astral-sh/ty/issues/1673
synced_at: 2026-01-12T15:54:25Z
```

# Show bounds or constraints of `T` when hovering over a variable that has type `T`

---

_@AlexWaygood_

When hovering over a variable that has type `T`, ty currently tells you the variance of `T`, which is really useful. It would also be very useful to know what the upper bound or constraints of `T` are, however. E.g. here, `T@f` has an upper bound of `F`.

https://github.com/user-attachments/assets/3202620f-8819-4ed1-8948-d2bfafd07d96

---

_Label `server` added by @AlexWaygood on 2025-11-29 15:21_

---

_Label `generics` added by @AlexWaygood on 2025-11-29 15:21_

---
