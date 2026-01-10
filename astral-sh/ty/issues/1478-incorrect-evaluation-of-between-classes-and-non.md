```yaml
number: 1478
title: "Incorrect evaluation of `|` between classes and non-classes"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2025-11-04T14:48:39Z
updated_at: 2025-11-06T14:00:44Z
url: https://github.com/astral-sh/ty/issues/1478
synced_at: 2026-01-10T02:06:25Z
```

# Incorrect evaluation of `|` between classes and non-classes

---

_Issue opened by @AlexWaygood on 2025-11-04 14:48_

### Summary

The special cases added in https://github.com/astral-sh/ruff/pull/21195 are a little too broad. Here's our behaviour on `main`:

```py
class Foo:
    def __ror__(self, other) -> str:
        return "foo"

reveal_type(int | Foo())  # `UnionType`, but should be `str`
reveal_type(int | 42)  # `UnionType` (and no diagnostic), but this fails at runtime
```

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-11-04 14:48_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-11-04 14:50_

---

_Closed by @AlexWaygood on 2025-11-06 14:00_

---
