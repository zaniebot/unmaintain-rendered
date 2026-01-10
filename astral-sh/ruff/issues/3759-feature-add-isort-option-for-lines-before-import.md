---
number: 3759
title: "[Feature] Add isort option for lines-before-import"
type: issue
state: open
author: Spectre5
labels:
  - isort
assignees: []
created_at: 2023-03-27T17:21:19Z
updated_at: 2025-02-06T15:14:12Z
url: https://github.com/astral-sh/ruff/issues/3759
synced_at: 2026-01-10T01:22:42Z
---

# [Feature] Add isort option for lines-before-import

---

_Issue opened by @Spectre5 on 2023-03-27 17:21_

Can we get this option added to ruff?

https://pycqa.github.io/isort/docs/configuration/options#lines-before-imports

---

_Label `isort` added by @charliermarsh on 2023-03-27 17:26_

---

_Referenced in [astral-sh/ruff#6190](../../astral-sh/ruff/issues/6190.md) on 2023-07-31 09:46_

---

_Referenced in [astral-sh/ruff#6281](../../astral-sh/ruff/issues/6281.md) on 2023-08-14 20:22_

---

_Referenced in [astral-sh/ruff#7026](../../astral-sh/ruff/issues/7026.md) on 2023-09-06 02:52_

---

_Comment by @brody4hire on 2023-09-18 23:27_

__UPDATED:__ for this example - _see new workaround below_:

```py
# import-sort-test.py

import sys as _sys

from urllib.error import URLError
```

`ruff check --select I001 import-sort-test.py` fails with an error ❌

`isort --check --lines-before-imports=1 import-sort-test.py` passes ✅

__New WORKAROUND SOLUTION:__

configure in `pyproject.toml`:
```toml
[tool.ruff.isort]
lines-between-types = 1
```

`ruff check --select I001 import-sort-test.py` now succeeds ✅

ref: https://docs.astral.sh/ruff/settings/#isort-lines-between-types

---

<details>
<summary><i>outdated workaround NOT RECOMMENDED</i></summary>

```py
# import-sort-test-with-comments.py

# global imports
import sys as _sys

# individual imports
from urllib.error import URLError
```
</details>

---

_Comment by @Satge96 on 2025-02-06 15:14_

For me, the workaround does not fix the problems shown in #6281 and #7026, that the number of lines between the docstring and the first import is not fixable to a specific value. Am I wrong?
I think this would be extremely helpful to make sure the files have the same structure.



---
