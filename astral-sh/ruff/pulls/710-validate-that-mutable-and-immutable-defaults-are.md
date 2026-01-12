```yaml
number: 710
title: Validate that mutable and immutable defaults are imported
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/imports
created_at: 2022-11-12T21:32:07Z
updated_at: 2022-11-12T21:45:38Z
url: https://github.com/astral-sh/ruff/pull/710
synced_at: 2026-01-12T15:55:05Z
```

# Validate that mutable and immutable defaults are imported

---

_@charliermarsh_

This ensures that if you `from fastapi import Query`, and `fastapi.Query` is listed as an extra immutable func, we don't error.

\cc @edgarrmondragon 

---

_Merged by @charliermarsh on 2022-11-12 21:32_

---

_Closed by @charliermarsh on 2022-11-12 21:32_

---

_Branch deleted on 2022-11-12 21:32_

---

_Comment by @edgarrmondragon on 2022-11-12 21:40_

Ah, this is a nice improvement over the original flake8-bugbear behavior 😁

Thanks @charliermarsh 

---

_Comment by @charliermarsh on 2022-11-12 21:45_

Thanks for the implementation! I had this TODOs around anyway so felt like a good time to knock them out. Note that the logic isn't _perfect_, e.g., it doesn't handle shadowing, like:

```py
from fastapi import Query
from other_library import Query
```

But, I mean, at that point we're nearly writing a Python runtime.


---
