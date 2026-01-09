---
number: 376
title: False positive on unused import in type annotation
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-10-09T21:27:45Z
updated_at: 2022-10-10T09:09:15Z
url: https://github.com/astral-sh/ruff/issues/376
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive on unused import in type annotation

---

_Issue opened by @charliermarsh on 2022-10-09 21:27_

```py
import sys

import numpy as np

if sys.version_info >= (3, 10):
    from typing import TypeAlias
else:
    from typing_extensions import TypeAlias


CustomInt: TypeAlias = "np.int8 | np.int16"
```

---

_Comment by @charliermarsh on 2022-10-09 21:28_

\cc @ghuls

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-09 21:28_

---

_Label `bug` added by @charliermarsh on 2022-10-09 21:28_

---

_Referenced in [astral-sh/ruff#377](../../astral-sh/ruff/pulls/377.md) on 2022-10-09 21:37_

---

_Closed by @charliermarsh on 2022-10-09 21:37_

---

_Comment by @ghuls on 2022-10-09 21:52_

When there is a `from __future__ import annotations` line before, it doesn't work yet.

```python
from __future__ import annotations

import sys

import numpy as np

if sys.version_info >= (3, 10):
    from typing import TypeAlias
else:
    from typing_extensions import TypeAlias


CustomInt: TypeAlias = "np.int8 | np.int16"
```

---

_Comment by @charliermarsh on 2022-10-09 22:13_

Good call. Will fix that too.

---

_Comment by @charliermarsh on 2022-10-09 22:28_

Should be resolved by #378.

---

_Comment by @ghuls on 2022-10-10 09:09_

Is fixed indeed! Thanks.

---
