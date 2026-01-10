```yaml
number: 1602
title: Comments are getting shifted on imports
type: issue
state: closed
author: blink1073
labels:
  - bug
  - isort
assignees: []
created_at: 2023-01-03T15:10:02Z
updated_at: 2023-01-04T12:43:25Z
url: https://github.com/astral-sh/ruff/issues/1602
synced_at: 2026-01-10T11:09:43Z
```

# Comments are getting shifted on imports

---

_Issue opened by @blink1073 on 2023-01-03 15:10_

I believe #1562 introduced a regression for us in [jupyter-server/jupyter_server_terminals@`b536f62` (#73)](https://github.com/jupyter-server/jupyter_server_terminals/pull/73/commits/b536f6252b138fcffcbb0bb61b38ed8faf2476f6).

We had formerly used:

```python
from jupyter_server.prometheus.metrics import (  # type:ignore[attr-defined]
    TERMINAL_CURRENTLY_RUNNING_TOTAL,
)
```

But it now gets mangled to:

```python
from jupyter_server.prometheus.metrics import (
    TERMINAL_CURRENTLY_RUNNING_TOTAL,  # type:ignore[attr-defined]
)
```

Which results in a mypy error.


---

_Label `isort` added by @charliermarsh on 2023-01-03 15:12_

---

_Comment by @charliermarsh on 2023-01-03 15:12_

Thanks! Will take a look.

---

_Label `bug` added by @charliermarsh on 2023-01-03 15:12_

---

_Comment by @charliermarsh on 2023-01-03 15:26_

Looks like it was introduced in #1380.

---

_Comment by @charliermarsh on 2023-01-03 15:28_

I'll fix this today (but probably not for a few hours).

---

_Comment by @blink1073 on 2023-01-03 15:31_

No worries, we have a work-around for now.  Thanks!

---

_Comment by @charliermarsh on 2023-01-03 15:31_

Thanks for filing :)

---

_Comment by @charliermarsh on 2023-01-03 19:27_

\cc @squiddy in case he gets to it first

---

_Closed by @charliermarsh on 2023-01-04 01:02_

---

_Comment by @blink1073 on 2023-01-04 12:43_

Thanks again!

---
