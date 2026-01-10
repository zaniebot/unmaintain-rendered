```yaml
number: 2255
title: Cache wheel metadata in no-PEP 658 fallback
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2024-03-07T00:41:00Z
updated_at: 2024-03-07T00:46:25Z
url: https://github.com/astral-sh/uv/pull/2255
synced_at: 2026-01-10T14:54:43Z
```

# Cache wheel metadata in no-PEP 658 fallback

---

_Pull request opened by @charliermarsh on 2024-03-07 00:41_

## Summary

If we fallback to streaming the wheel (because the registry doesn't support range requests), we currently don't cache the metadata at all. This PR fixes that, ensuring that we cache based on the same HTTP policies, etc.

---

_Label `performance` added by @charliermarsh on 2024-03-07 00:41_

---

_Marked ready for review by @charliermarsh on 2024-03-07 00:41_

---

_Merged by @charliermarsh on 2024-03-07 00:46_

---

_Closed by @charliermarsh on 2024-03-07 00:46_

---

_Branch deleted on 2024-03-07 00:46_

---
