---
number: 4346
title: "[ruff.isort] Wrapping imports"
type: issue
state: closed
author: zombeer
labels: []
assignees: []
created_at: 2023-05-10T14:41:22Z
updated_at: 2023-05-10T14:53:08Z
url: https://github.com/astral-sh/ruff/issues/4346
synced_at: 2026-01-10T01:22:43Z
---

# [ruff.isort] Wrapping imports

---

_Issue opened by @zombeer on 2023-05-10 14:41_

Hi!
Can you please suggest an option to get result like:
```python
from __future__ import absolute_import

import os
import sys

from my_lib import Object, Object2, Object3
from third_party import (lib1, lib2, lib3, lib4, lib5, lib6, lib7, lib8, lib9,
                         lib10, lib11, lib12, lib13, lib14, lib15)

print("Hey")
print("yo")

```
While I keep getting:
```python
from __future__ import absolute_import

import os
import sys

from my_lib import Object, Object2, Object3
from third_party import (
    lib1,
    lib2,
    lib3,
    lib4,
    lib5,
    lib6,
    lib7,
    lib8,
    lib9,
    lib10,
    lib11,
    lib12,
    lib13,
    lib14,
    lib15,
)

print("Hey")
print("yo")
```

---

_Comment by @charliermarsh on 2023-05-10 14:53_

We don't currently support these wrapping formats -- we use the Black-style formatting. There's an existing issue here (#2600) to track that work, though it's not currently being worked on.

---

_Closed by @charliermarsh on 2023-05-10 14:53_

---
