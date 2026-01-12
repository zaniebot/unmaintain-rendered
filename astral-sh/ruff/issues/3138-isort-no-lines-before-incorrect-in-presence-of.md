```yaml
number: 3138
title: isort no-lines-before incorrect in presence of preceding empty sections
type: issue
state: closed
author: bluetech
labels: []
assignees: []
created_at: 2023-02-22T19:14:48Z
updated_at: 2023-02-22T19:35:55Z
url: https://github.com/astral-sh/ruff/issues/3138
synced_at: 2026-01-12T15:54:43Z
```

# isort no-lines-before incorrect in presence of preceding empty sections

---

_@bluetech_

Version: 0.0.251

Consider imports such as

```py
from __future__ import print  # future
from django import db         # third-party
from . import y               # local-folder
```

and isort config `no-lines-before = ["local-folder"]`.

Expected behavior is:

```py
from __future__ import print  # future

from django import db         # third-party

from . import y               # local-folder
```

Even though we specified no lines before `local-folder`, there should still be a line before in this case, because the `first-party` section which comes between `local-folder` and `third-party` *isn't* included in `no-lines-before`. This is what `isort` does.

Actual behavior:

```py
from __future__ import print  # future

from django import db         # third-party
from . import y               # local-folder
```

---

_Closed by @charliermarsh on 2023-02-22 19:35_

---
