```yaml
number: 2021
title: SIM102 walrus operator
type: issue
state: closed
author: Kludex
labels:
  - bug
  - question
assignees: []
created_at: 2023-01-20T10:53:39Z
updated_at: 2023-01-20T12:18:36Z
url: https://github.com/astral-sh/ruff/issues/2021
synced_at: 2026-01-12T15:54:42Z
```

# SIM102 walrus operator

---

_@Kludex_

# Description

SIM102 is being raised when using walrus operator.

## Code

```python
def something(schema: dict) -> None:
    if all_of := schema.get("allOf"):
        if len(all_of) == 1:
            schema.update(all_of[0])
            del schema["allOf"]


something({"allOf": [{"type": "string"}]})
something({})
```

## Output

```bash
main.py:2:5: SIM102 Use a single `if` statement instead of nested `if` statements
```

## Notes

An alternative code for the above would be:
```python
def something(schema: dict) -> None:
    all_of = schema.get("allOf")
    if all_of and len(all_of) == 1:
        schema.update(all_of[0])
        del schema["allOf"]


something({"allOf": [{"type": "string"}]})
something({})
```
Which passes the linter.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-20 12:10_

---

_Label `bug` added by @charliermarsh on 2023-01-20 12:10_

---

_Comment by @charliermarsh on 2023-01-20 12:17_

Thanks for filing!

I think this actually does produce valid code -- running `ruff --isolated --select SIM102 --fix` on that snippet gives me:

```py
def something(schema: dict) -> None:
    if (all_of := schema.get("allOf")) and len(all_of) == 1:
        schema.update(all_of[0])
        del schema["allOf"]


something({"allOf": [{"type": "string"}]})
something({})
```

Which seems to run ok? As long as the walrus is parenthesized. Did you run into another example where this fix produced invalid code? (Or is the issue to suggest that, stylistically, this is worse than before?)


---

_Label `question` added by @charliermarsh on 2023-01-20 12:17_

---

_Comment by @Kludex on 2023-01-20 12:18_

Ah! This is fine. Thanks 🙏

---

_Closed by @Kludex on 2023-01-20 12:18_

---
