---
number: 1252
title: "Support isort's force_single_line option"
type: issue
state: closed
author: brutus
labels:
  - isort
  - configuration
assignees: []
created_at: 2022-12-15T21:15:42Z
updated_at: 2022-12-27T13:51:33Z
url: https://github.com/astral-sh/ruff/issues/1252
synced_at: 2026-01-07T13:12:14-06:00
---

# Support isort's force_single_line option

---

_Issue opened by @brutus on 2022-12-15 21:15_

Consider adding support for [isort's `force_single_line` option](https://pycqa.github.io/isort/docs/configuration/options.html#force-single-line). 

I.e.

```python
from pathlib import Path, PurePath
```

becomes 

```python
from pathlib import Path
from pathlib import PurePath
```

---

_Label `isort` added by @charliermarsh on 2022-12-15 22:02_

---

_Label `configuration` added by @charliermarsh on 2022-12-15 22:02_

---

_Referenced in [astral-sh/ruff#1366](../../astral-sh/ruff/pulls/1366.md) on 2022-12-24 20:22_

---

_Closed by @charliermarsh on 2022-12-27 13:51_

---
