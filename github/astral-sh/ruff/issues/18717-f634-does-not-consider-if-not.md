---
number: 18717
title: F634 does not consider if-not
type: issue
state: open
author: fnep
labels:
  - rule
assignees: []
created_at: 2025-06-17T07:52:47Z
updated_at: 2025-06-17T17:50:37Z
url: https://github.com/astral-sh/ruff/issues/18717
synced_at: 2026-01-07T13:12:16-06:00
---

# F634 does not consider if-not

---

_Issue opened by @fnep on 2025-06-17 07:52_

### Summary

The F634 rule is great to find issues with tuples created in an if condition, leading to always true conditions:

```python3
if (False,):  # F634 If test is a tuple, which is always `True`
    print("This will always run")
```

It would be great if it could also catch the very similar if-not case:

```python3
if not (True,):
    print("This will never run")
```

In my codebase, this sometimes leads to bugs where we check the result length and return a 404 if there is no result.

Example:

```python3
if not (
    item_list := get_items(
        foo=bar,
    ),
):
    raise HTTPException(status_code=404, detail="No items found")

# ... continue with item_list
```

Here it would be great to get a warning.


---

_Label `rule` added by @ntBre on 2025-06-17 17:50_

---
