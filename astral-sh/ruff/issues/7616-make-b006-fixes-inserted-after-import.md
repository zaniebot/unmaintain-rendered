```yaml
number: 7616
title: Make B006 fixes inserted after import
type: issue
state: closed
author: hoxbro
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-09-23T12:52:38Z
updated_at: 2023-09-27T01:26:30Z
url: https://github.com/astral-sh/ruff/issues/7616
synced_at: 2026-01-10T11:09:49Z
```

# Make B006 fixes inserted after import

---

_Issue opened by @hoxbro on 2023-09-23 12:52_

When running `ruff --select=B006 b006.py --fix` on a function like this
``` python
def test(arg=[]):
    import os
```

the fix would be:
``` python
def test(arg=None):
    if arg is None:
        arg = []
    import os
```

I would like to see the fix move below the import like this:
``` python
def test(arg=None):
    import os
    if arg is None:
        arg = []
```

For my own enjoyment, I have already started implementing this [here](https://github.com/Hoxbro/ruff/pull/1). Though, as this is the first time I have written any Rust, it could likely be improved upon :sweat_smile:   


---

_Label `rule` added by @charliermarsh on 2023-09-23 16:01_

---

_Label `accepted` added by @charliermarsh on 2023-09-23 16:01_

---

_Comment by @charliermarsh on 2023-09-23 16:01_

This makes sense to me!

---

_Comment by @hoxbro on 2023-09-23 16:10_

Awesome, can you assign it to me?

---

_Assigned to @hoxbro by @charliermarsh on 2023-09-23 16:43_

---

_Closed by @charliermarsh on 2023-09-27 01:26_

---
