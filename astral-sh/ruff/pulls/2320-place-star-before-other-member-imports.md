```yaml
number: 2320
title: Place star before other member imports
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star-import
created_at: 2023-01-29T03:14:44Z
updated_at: 2023-01-29T03:17:45Z
url: https://github.com/astral-sh/ruff/pull/2320
synced_at: 2026-01-12T04:52:00Z
```

# Place star before other member imports

---

_Pull request opened by @charliermarsh on 2023-01-29 03:14_

I think we've never run into this case, since it's rare to import `*` from a module _and_ import some other member explicitly. But we were deviating from `isort` by placing the `*` after other members, rather than up-top.

Closes #2318.


---

_Merged by @charliermarsh on 2023-01-29 03:17_

---

_Closed by @charliermarsh on 2023-01-29 03:17_

---

_Branch deleted on 2023-01-29 03:17_

---
