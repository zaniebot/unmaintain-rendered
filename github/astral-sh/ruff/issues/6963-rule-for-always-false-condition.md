---
number: 6963
title: rule for always false condition
type: issue
state: open
author: tooptoop4
labels:
  - rule
assignees: []
created_at: 2023-08-29T02:57:52Z
updated_at: 2023-09-13T18:25:42Z
url: https://github.com/astral-sh/ruff/issues/6963
synced_at: 2026-01-07T13:12:15-06:00
---

# rule for always false condition

---

_Issue opened by @tooptoop4 on 2023-08-29 02:57_

new_header is an int type
below condition can never be satisfied, any rule to highlight this?

`if new_header and new_header == 0:`


---

_Comment by @charliermarsh on 2023-08-30 18:02_

We don't have a rule to capture this right now. We could extend https://beta.ruff.rs/docs/rules/expr-and-not-expr/ to handle some of these cases. I consider it low-priority.

---

_Label `rule` added by @charliermarsh on 2023-08-30 18:02_

---

_Comment by @dosisod on 2023-09-13 18:25_

I've ran into similar situations where I have the following code:

```python
def is_allowed_extension(filename: str) -> bool:
    return filename.upper().endswith((".png", "jpg"))
```

Obviously because I called `upper()` it's impossible for the string to contain lower-case strings. I've definitely been fooled by this in the past, though I usually figure it out after a few minutes. I think it would be novel if Ruff could detect stuff like this, it might save you the trouble of debugging something that could be statically detected.

---
