```yaml
number: 5666
title: Remove lingering executables after failed installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/linger
created_at: 2024-07-31T18:57:28Z
updated_at: 2024-07-31T19:27:26Z
url: https://github.com/astral-sh/uv/pull/5666
synced_at: 2026-01-12T16:06:57Z
```

# Remove lingering executables after failed installs

---

_@charliermarsh_

## Summary

This could still be made more robust, but it's not critical, since you can always `--force`. It's good to handle this case, though, since we have an explicit error for it.

Closes https://github.com/astral-sh/uv/issues/5490.


---

_Label `bug` added by @charliermarsh on 2024-07-31 18:57_

---

_@zanieb approved on 2024-07-31 19:04_

---

_Merged by @charliermarsh on 2024-07-31 19:27_

---

_Closed by @charliermarsh on 2024-07-31 19:27_

---

_Branch deleted on 2024-07-31 19:27_

---
