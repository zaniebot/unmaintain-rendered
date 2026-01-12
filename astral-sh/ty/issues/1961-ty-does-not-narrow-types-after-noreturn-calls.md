```yaml
number: 1961
title: ty does not narrow types after NoReturn calls
type: issue
state: closed
author: seangies
labels: []
assignees: []
created_at: 2025-12-16T22:21:14Z
updated_at: 2025-12-16T22:25:52Z
url: https://github.com/astral-sh/ty/issues/1961
synced_at: 2026-01-12T15:54:26Z
```

# ty does not narrow types after NoReturn calls

---

_@seangies_

### Summary

`ty` does not handle type-narrowing after calls to `NoReturn` functions like `sys.exit()`:

```
❯ uvx ty check /tmp/ty-noreturn-bug.py
warning[possibly-missing-attribute]: Attribute `get` may be missing on object of type `dict[Unknown, Unknown] | None`
  --> /tmp/ty-noreturn-bug.py:21:13
   |
19 |     # ty warns here: "Attribute `get` may be missing on object of type `dict | None`"
20 |     # even though sys.exit(1) above means result cannot be None here
21 |     value = result.get("key")
   |             ^^^^^^^^^^
22 |     print(value)
   |
info: rule `possibly-missing-attribute` is enabled by default

Found 1 diagnostic
```

[ty-noreturn-bug.py](https://github.com/user-attachments/files/24200812/ty-noreturn-bug.py)

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @carljm on 2025-12-16 22:25_

Thanks for the report! This is a known issue, tracked at #690, and a top priority to fix.

---

_Closed by @carljm on 2025-12-16 22:25_

---
