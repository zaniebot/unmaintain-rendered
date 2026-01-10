```yaml
number: 9538
title: Avoid using IDs across PubGrub states
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2024-11-30T13:50:32Z
updated_at: 2024-11-30T13:59:22Z
url: https://github.com/astral-sh/uv/pull/9538
synced_at: 2026-01-10T12:00:00Z
```

# Avoid using IDs across PubGrub states

---

_Pull request opened by @charliermarsh on 2024-11-30 13:50_

## Summary

This isn't safe, because the prefetcher is global but the IDs could come from different PubGrub states (i.e., different forks).


---

_Label `bug` added by @charliermarsh on 2024-11-30 13:50_

---

_Marked ready for review by @charliermarsh on 2024-11-30 13:50_

---

_Merged by @charliermarsh on 2024-11-30 13:59_

---

_Closed by @charliermarsh on 2024-11-30 13:59_

---

_Branch deleted on 2024-11-30 13:59_

---
