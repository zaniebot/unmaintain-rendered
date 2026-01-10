---
number: 19797
title: Add a rule for unused functions
type: issue
state: closed
author: mat926
labels: []
assignees: []
created_at: 2025-08-07T02:23:00Z
updated_at: 2025-08-07T11:53:26Z
url: https://github.com/astral-sh/ruff/issues/19797
synced_at: 2026-01-10T01:23:00Z
---

# Add a rule for unused functions

---

_Issue opened by @mat926 on 2025-08-07 02:23_

### Summary

Hello can Ruff add a rule when a function or method is not used anywhere? 

Example:
```python
def used_function():
    print("This function is used.")

def unused_function():
    print("This function is NOT used.")

used_function()


```

---

_Comment by @ntBre on 2025-08-07 11:53_

I believe this is tracked in #872, but it does sound useful!

---

_Closed by @ntBre on 2025-08-07 11:53_

---
