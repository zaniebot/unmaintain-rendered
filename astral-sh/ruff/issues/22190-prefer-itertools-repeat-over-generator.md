```yaml
number: 22190
title: "Prefer `itertools.repeat` over generator expressions (`foo for _ in range(n)`)"
type: issue
state: open
author: injust
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-25T06:38:38Z
updated_at: 2025-12-25T14:42:43Z
url: https://github.com/astral-sh/ruff/issues/22190
synced_at: 2026-01-12T15:54:58Z
```

# Prefer `itertools.repeat` over generator expressions (`foo for _ in range(n)`)

---

_@injust_

### Summary

`itertools.repeat(foo, n)` is usually clearer and easier to read than the corresponding generator expression `foo for _ in range(n)`.

A case where it might be less readable is when you are repeating an integer, i.e. `itertools.repeat(3, 2)`. It isn't immediately clear whether this is 3 repeated twice, or 2 repeated 3 times.

https://github.com/astral-sh/ruff/issues/22189 is similar in a way, but specifically for list comprehensions.

---

_Label `rule` added by @ntBre on 2025-12-25 14:42_

---

_Label `needs-decision` added by @ntBre on 2025-12-25 14:42_

---
