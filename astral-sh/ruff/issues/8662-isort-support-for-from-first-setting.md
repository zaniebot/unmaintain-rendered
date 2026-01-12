```yaml
number: 8662
title: "isort: support for ``from_first`` setting"
type: issue
state: closed
author: jelmer
labels:
  - isort
assignees: []
created_at: 2023-11-13T19:07:18Z
updated_at: 2023-11-21T23:36:16Z
url: https://github.com/astral-sh/ruff/issues/8662
synced_at: 2026-01-12T15:54:48Z
```

# isort: support for ``from_first`` setting

---

_@jelmer_

isort has a ``from_first`` setting that forces "from .. import .." lines to appear before "import .." lines.

It would be great if ruff could support this too, or something like it. For example:

```
$ isort --ff true x.py
```

results in:

```python
from __future__ import annotations

from os import path
import enum
import os
```

---

_Label `isort` added by @charliermarsh on 2023-11-13 20:45_

---

_Closed by @charliermarsh on 2023-11-21 23:36_

---
