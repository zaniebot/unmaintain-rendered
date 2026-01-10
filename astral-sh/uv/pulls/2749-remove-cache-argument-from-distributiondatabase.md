```yaml
number: 2749
title: "Remove `Cache` argument from `DistributionDatabase`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2024-04-01T02:08:58Z
updated_at: 2024-04-01T02:22:26Z
url: https://github.com/astral-sh/uv/pull/2749
synced_at: 2026-01-10T14:49:08Z
```

# Remove `Cache` argument from `DistributionDatabase`

---

_Pull request opened by @charliermarsh on 2024-04-01 02:08_

## Summary

We can access cache from `BuildContext`. This mirrors `SourceDistCachedBuilder`, which doesn't accept `Cache` as an argument and always accesses it through `BuildContext`.


---

_Label `internal` added by @charliermarsh on 2024-04-01 02:09_

---

_Marked ready for review by @charliermarsh on 2024-04-01 02:22_

---

_Merged by @charliermarsh on 2024-04-01 02:22_

---

_Closed by @charliermarsh on 2024-04-01 02:22_

---

_Branch deleted on 2024-04-01 02:22_

---
