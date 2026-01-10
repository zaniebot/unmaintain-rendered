```yaml
number: 3655
title: Ruff ignores nested string annotations
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-21T20:24:44Z
updated_at: 2023-03-26T01:56:11Z
url: https://github.com/astral-sh/ruff/issues/3655
synced_at: 2026-01-10T11:09:46Z
```

# Ruff ignores nested string annotations

---

_Issue opened by @charliermarsh on 2023-03-21 20:24_

Mypy respects the use of `Path` here:

```python
from pathlib import Path

x: """list['Path']""" = []
```

We consider it an unused import.


---

_Label `bug` added by @charliermarsh on 2023-03-21 20:24_

---

_Comment by @charliermarsh on 2023-03-21 20:34_

There's no reason (AFAIK) to do this as a user, but it _is_ valid.

---

_Closed by @charliermarsh on 2023-03-26 01:56_

---
