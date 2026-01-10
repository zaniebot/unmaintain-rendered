```yaml
number: 1319
title: "Incorrect handling of `list[LiteralString]`"
type: issue
state: closed
author: eteran
labels:
  - bidirectional inference
assignees: []
created_at: 2025-10-07T17:09:12Z
updated_at: 2025-10-07T17:21:03Z
url: https://github.com/astral-sh/ty/issues/1319
synced_at: 2026-01-10T02:06:25Z
```

# Incorrect handling of `list[LiteralString]`

---

_Issue opened by @eteran on 2025-10-07 17:09_

### Summary

There appears to be incorrect handling of `list[LiteralString]`.

The following code triggers an invalid-assignment error:

```python
example: list[LiteralString] = [
    "this is a test",
    "for a list of string literals",
]
```

> error[invalid-assignment]: Object of type `list[str]` is not assignable to `list[LiteralString]`.

I believe the tool should be able to correctly identify that each element is in fact a `LiteralString`.

### Version

ty 0.0.1-alpha.21

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-10-07 17:15_

---

_Comment by @AlexWaygood on 2025-10-07 17:21_

Thanks for the report. This is a duplicate of #1198

---

_Closed by @AlexWaygood on 2025-10-07 17:21_

---
