```yaml
number: 5571
title: Skip copying to empty entries in seekable zip
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/skip-zero
created_at: 2024-07-29T18:52:19Z
updated_at: 2024-07-29T19:00:20Z
url: https://github.com/astral-sh/uv/pull/5571
synced_at: 2026-01-12T16:06:54Z
```

# Skip copying to empty entries in seekable zip

---

_@charliermarsh_

## Summary

We cannot do this when streaming, since we may not have the metadata for the entry.

Closes https://github.com/astral-sh/uv/issues/5565.


---

_Label `performance` added by @charliermarsh on 2024-07-29 18:52_

---

_Merged by @charliermarsh on 2024-07-29 19:00_

---

_Closed by @charliermarsh on 2024-07-29 19:00_

---

_Branch deleted on 2024-07-29 19:00_

---
