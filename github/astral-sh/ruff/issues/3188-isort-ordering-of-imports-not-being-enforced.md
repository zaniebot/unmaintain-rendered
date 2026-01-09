---
number: 3188
title: "`isort` ordering of imports not being enforced."
type: issue
state: closed
author: brysontyrrell
labels: []
assignees: []
created_at: 2023-02-23T19:28:59Z
updated_at: 2023-02-23T19:36:09Z
url: https://github.com/astral-sh/ruff/issues/3188
synced_at: 2026-01-07T13:12:14-06:00
---

# `isort` ordering of imports not being enforced.

---

_Issue opened by @brysontyrrell on 2023-02-23 19:28_

I'm experimenting replacing `isort` with `ruff` but it doesn't seem to be enforcing the order of imports even if I explicitly set `order-by-type = true`.

Here's what is in `pyproject.toml`:

```toml
[tool.ruff]
line-length = 100
target-version = "py39"
src = [
    "src",
    "test"
]


[tool.ruff.isort]
known-first-party = [ "my-package" ]
order-by-type = true
force-sort-within-sections = true
```

Here's the test Python file `my-package/__init__.py`:
```python
from .shared import IMPORT_ME  # first-party

import json  # standard lib
import requests  # third party


print(requests)
print(json)
print(IMPORT_ME)
```

`isort` properly returns an error when it checks this file:
```
ERROR: /path/to/my-package/__init__.py Imports are incorrectly sorted and/or formatted
```

Ruff is at version `0.0.252`.

---

_Closed by @brysontyrrell on 2023-02-23 19:34_

---

_Comment by @brysontyrrell on 2023-02-23 19:34_

Ignore that I opened this...

---

_Comment by @charliermarsh on 2023-02-23 19:35_

All good! Is it just that you needed to enable the `I` rules?

---

_Comment by @brysontyrrell on 2023-02-23 19:35_

I had it in the wrong place in the pyproject file 🤦‍♂️ Saw it after I hit save on the issue.

---

_Comment by @charliermarsh on 2023-02-23 19:36_

Happens to the best of us :D

---
