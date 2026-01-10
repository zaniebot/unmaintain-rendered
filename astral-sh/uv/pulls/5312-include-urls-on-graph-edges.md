```yaml
number: 5312
title: Include URLs on graph edges
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/edge
created_at: 2024-07-22T21:12:29Z
updated_at: 2024-07-22T22:41:04Z
url: https://github.com/astral-sh/uv/pull/5312
synced_at: 2026-01-10T13:37:23Z
```

# Include URLs on graph edges

---

_Pull request opened by @charliermarsh on 2024-07-22 21:12_

## Summary

Excellent find from @konstin. If we have a package that's included in two forks at the same version, but with different URLs, we need to avoid collapsing them in the lockfile.

Closes https://github.com/astral-sh/uv/issues/5294.


---

_Label `bug` added by @charliermarsh on 2024-07-22 21:12_

---

_Comment by @charliermarsh on 2024-07-22 21:31_

This failure is on main already, so gonna merge.

---

_Comment by @charliermarsh on 2024-07-22 21:31_

Wait, nevermind, there's a separate Windows failure.

---

_Merged by @charliermarsh on 2024-07-22 22:41_

---

_Closed by @charliermarsh on 2024-07-22 22:41_

---

_Branch deleted on 2024-07-22 22:41_

---
