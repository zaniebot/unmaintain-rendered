```yaml
number: 8923
title: "`boolean-positional-value-in-call` (`FBT003`) - don't warn when function name starts with `set_` and only takes one argument"
type: issue
state: closed
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2023-11-30T12:01:00Z
updated_at: 2024-05-04T21:33:09Z
url: https://github.com/astral-sh/ruff/issues/8923
synced_at: 2026-01-12T15:54:48Z
```

# `boolean-positional-value-in-call` (`FBT003`) - don't warn when function name starts with `set_` and only takes one argument

---

_@DetachHead_

functions like these should not report the error imo, since the meaning of the bool is self explanatory, which is often the case when the functin starts with `set_`:

```py
def set_enabled(enabled: bool):
    ...

set_enabled(True) # self explanatory
set_enabled(enabled=True) # unnecessary/redundant
```

---

_Comment by @zanieb on 2023-11-30 14:57_

This seems a little bit too specific to me.

---

_Label `rule` added by @charliermarsh on 2024-01-08 03:44_

---

_Comment by @charliermarsh on 2024-01-08 03:46_

We may want to do this, it would also help with https://github.com/astral-sh/ruff/issues/9287 and https://github.com/astral-sh/ruff/issues/3247#issuecomment-1842529367.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-08 04:25_

---

_Closed by @charliermarsh on 2024-01-08 18:02_

---

_Comment by @tmke8 on 2024-01-18 14:15_

Maybe also allow the `use_` prefix? I encountered that with [torch.use_deterministic_algorithms](https://pytorch.org/docs/stable/generated/torch.use_deterministic_algorithms.html). The following feels a bit silly:

```python
torch.use_deterministic_algorithms(mode=True)
```

---

_Comment by @charliermarsh on 2024-01-18 16:04_

I'm kind of wondering if we should just avoid calling this on third-party function calls.

---

_Comment by @johnslavik on 2024-04-16 23:03_

I often use flag-holding context vars.
```py
from contextvars import ContextVar

cv = ContextVar("cv", default=False)
cv.set(True)  # triggers FBT003
```

What a pity the fix for this doesn't apply to `ContextVar.set`.

---

_Comment by @DetachHead on 2024-04-16 23:32_

it looks like the check expects the function name to start with "set" followed by either an underscore or an uppercase character, but not "set" on its own. it should probably be updated to allow that too.

---

_Comment by @zanieb on 2024-04-16 23:36_

It seems reasonable to expand it to cover that case.

---

_Reopened by @zanieb on 2024-04-16 23:36_

---

_Closed by @charliermarsh on 2024-05-04 21:33_

---
