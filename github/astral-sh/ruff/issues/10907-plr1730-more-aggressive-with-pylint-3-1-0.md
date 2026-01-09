---
number: 10907
title: PLR1730 more aggressive with pylint 3.1.0
type: issue
state: closed
author: xrmx
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-04-12T13:12:04Z
updated_at: 2024-04-13T22:57:22Z
url: https://github.com/astral-sh/ruff/issues/10907
synced_at: 2026-01-07T13:12:15-06:00
---

# PLR1730 more aggressive with pylint 3.1.0

---

_Issue opened by @xrmx on 2024-04-12 13:12_

With the following snippet:

```
class Foo:
    _min = 0

    def foo(self, value):
        if value < self._min:
            self._min = value                             
```

pylint 3.1.0 returns:

```
foo.py:6:8: R1730: Consider using '_min = min(_min, value)' instead of unnecessary if block (consider-using-min-builtin)
```

ruff 0.3.7 does not report it. pylint 3.0.2 did not report it too.

---

_Comment by @charliermarsh on 2024-04-12 13:27_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-04-12 13:27_

---

_Label `help wanted` added by @charliermarsh on 2024-04-12 13:27_

---

_Referenced in [astral-sh/ruff#10920](../../astral-sh/ruff/pulls/10920.md) on 2024-04-13 19:37_

---

_Closed by @charliermarsh on 2024-04-13 22:57_

---
