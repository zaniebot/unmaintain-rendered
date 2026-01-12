```yaml
number: 16107
title: "Rule to force `var = <default> if value is None else value` instead of `var = value or <default>` for `value: int | None = None`"
type: issue
state: open
author: adriangb
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2025-02-11T23:42:31Z
updated_at: 2025-02-12T08:30:06Z
url: https://github.com/astral-sh/ruff/issues/16107
synced_at: 2026-01-12T15:54:55Z
```

# Rule to force `var = <default> if value is None else value` instead of `var = value or <default>` for `value: int | None = None`

---

_@adriangb_

### Description

I often see the pattern of a function taking a parameter such as `value: int | None = None` and then having an internal default by doing `var = value or 1`. This is problematic because 0 and None are both falsy:

```python
def f(value: int | None) -> int:
    return value or 1

f(0)  # returns 1
```

I guess one might do this intentionally, but unless you are explicit the meaning is unclear. I would rather see one of:

```python
def f(value: int | None) -> int:
    return 1 if value is None else value

def f(value: int | None) -> int:
    return 1 if value is None or value == 0 else value
```

Arguably in these simple cases just making the parameter `value: int = 1` would be better, but I believe there are use cases for this pattern or it is at least a pattern that exists 

---

_Label `rule` added by @MichaReiser on 2025-02-12 08:28_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-12 08:28_

---

_Label `type-inference` added by @MichaReiser on 2025-02-12 08:30_

---
