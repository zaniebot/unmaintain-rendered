---
number: 3108
title: "E701: false positive since 0.0.250"
type: issue
state: closed
author: bastimeyer
labels:
  - bug
assignees: []
created_at: 2023-02-21T23:41:42Z
updated_at: 2023-02-22T15:44:48Z
url: https://github.com/astral-sh/ruff/issues/3108
synced_at: 2026-01-07T13:12:14-06:00
---

# E701: false positive since 0.0.250

---

_Issue opened by @bastimeyer on 2023-02-21 23:41_

**E701.py**
```py
from typing import Match, Optional


class Foo:
    match: Optional[Match] = None
```

```
$ ruff --version
ruff 0.0.250

$ ruff --select E E701.py 
E701.py:5:10: E701 Multiple statements on one line (colon)
Found 1 error.
```

No issues on `0.0.249`.

This looks like it's related to the `match` class attribute name. If this gets changed to something else, then no error gets raised.

---

_Label `bug` added by @charliermarsh on 2023-02-21 23:45_

---

_Comment by @charliermarsh on 2023-02-21 23:45_

Ohh yes. This makes sense. Thank you and sorry for the false positive.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-21 23:45_

---

_Comment by @charliermarsh on 2023-02-22 00:02_

Would be solved by https://github.com/RustPython/RustPython/pull/4542 plus a minor change in here.

---

_Referenced in [astral-sh/ruff#3129](../../astral-sh/ruff/pulls/3129.md) on 2023-02-22 15:39_

---

_Closed by @charliermarsh on 2023-02-22 15:44_

---
