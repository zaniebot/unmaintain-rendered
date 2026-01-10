```yaml
number: 6995
title: "False positive `PERF402` on `sys.path`"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
assignees: []
created_at: 2023-08-29T20:05:20Z
updated_at: 2023-08-29T23:15:30Z
url: https://github.com/astral-sh/ruff/issues/6995
synced_at: 2026-01-10T11:09:49Z
```

# False positive `PERF402` on `sys.path`

---

_Issue opened by @jamesbraza on 2023-08-29 20:05_

```python
import os
import sys

ONE_FOLDER_UP = os.path.join(os.path.dirname(__file__), "..")
for path in (ONE_FOLDER_UP,):  # Let's pretend there's many more than 1 item here
    sys.path.append(path)
```

With `ruff==0.0.286`:

```bash
> ruff --isolated --select=PERF402 quick_play.py
quick_play.py:6:5: PERF402 Use `list` or `list.copy` to create a copy of a list
Found 1 error.
```

I think this is a false positive `PERF402` because we shouldn't copy or set `sys.path`.

---

Aside: https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb113-use-list-extend (see https://github.com/astral-sh/ruff/issues/1348) makes more sense for use cases like this.

```python
sys.path.extend((ONE_FOLDER_UP,))
```


---

_Label `bug` added by @charliermarsh on 2023-08-29 20:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-29 21:34_

---

_Closed by @charliermarsh on 2023-08-29 23:15_

---
