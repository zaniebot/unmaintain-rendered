```yaml
number: 70
title: Avoid passing cached wheels to the resolver step
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/skip-resolve
created_at: 2023-10-09T02:02:04Z
updated_at: 2023-10-09T02:17:20Z
url: https://github.com/astral-sh/uv/pull/70
synced_at: 2026-01-12T16:03:43Z
```

# Avoid passing cached wheels to the resolver step

---

_@charliermarsh_

When we go to install a locked `requirements.txt`, if a wheel is already available in the local cache, and matches the version specifiers, we can just use it directly without fetching the package metadata. This speeds up the no-op case by about 33%.

Closes https://github.com/astral-sh/puffin/issues/48.


---

_Merged by @charliermarsh on 2023-10-09 02:17_

---

_Closed by @charliermarsh on 2023-10-09 02:17_

---

_Branch deleted on 2023-10-09 02:17_

---
