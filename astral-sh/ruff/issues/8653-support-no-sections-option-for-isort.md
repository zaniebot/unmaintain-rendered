---
number: 8653
title: support no_sections option for isort
type: issue
state: closed
author: jelmer
labels:
  - isort
assignees: []
created_at: 2023-11-13T15:05:39Z
updated_at: 2023-11-14T21:45:52Z
url: https://github.com/astral-sh/ruff/issues/8653
synced_at: 2026-01-10T01:22:48Z
---

# support no_sections option for isort

---

_Issue opened by @jelmer on 2023-11-13 15:05_

ruff's isort doesn't support the ``no_sections`` option that isort upstream has.  This option puts all import sinto the same import bucket.

For example, consider this source file:

```python
import os

import bar
import blah
```

isort --no-sections x.py format it as

```python
import bar
import blah
import os
```

As far as I can tell, there is no set of options for ruff's isort that provides this behaviour (if there is and I've missed it, I'll volunteer to add a note to the docs)

---

_Label `isort` added by @charliermarsh on 2023-11-13 15:23_

---

_Comment by @charliermarsh on 2023-11-13 15:23_

I don't believe we support this today.

---

_Comment by @jelmer on 2023-11-13 17:19_

FWIW I'm trying to come up with a PR for this.

---

_Referenced in [astral-sh/ruff#8657](../../astral-sh/ruff/pulls/8657.md) on 2023-11-13 17:39_

---

_Closed by @charliermarsh on 2023-11-14 21:45_

---
