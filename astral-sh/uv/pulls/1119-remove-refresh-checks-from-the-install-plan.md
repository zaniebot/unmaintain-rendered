```yaml
number: 1119
title: Remove refresh checks from the install plan
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fresh
created_at: 2024-01-26T03:39:09Z
updated_at: 2024-01-26T03:48:17Z
url: https://github.com/astral-sh/uv/pull/1119
synced_at: 2026-01-12T16:04:26Z
```

# Remove refresh checks from the install plan

---

_@charliermarsh_

## Summary

Rather than checking cache freshness in the install plan, it's a lot simple to have the install plan _never_ return cached data when the refresh policy is in place, and then rely on the distribution database to check for freshness. The original implementation didn't support this, since the distribution database was rebuilding things too often. Now, it rarely rebuilds (it's much better about this), so it seems conceptually much simpler to split up the responsibilities like this.


---

_Label `internal` added by @charliermarsh on 2024-01-26 03:48_

---

_Merged by @charliermarsh on 2024-01-26 03:48_

---

_Closed by @charliermarsh on 2024-01-26 03:48_

---

_Branch deleted on 2024-01-26 03:48_

---
