```yaml
number: 2419
title: Add backoff for transient Windows failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/backoff
created_at: 2024-03-13T16:55:29Z
updated_at: 2024-03-13T17:16:27Z
url: https://github.com/astral-sh/uv/pull/2419
synced_at: 2026-01-10T14:49:08Z
```

# Add backoff for transient Windows failures

---

_Pull request opened by @charliermarsh on 2024-03-13 16:55_

## Summary

This may be required elsewhere, but all the traces in that issue are related to persisting the temporary directory to our persistent cache, so lets start there.

See: https://github.com/astral-sh/uv/issues/1491.


---

_Review requested from @konstin by @charliermarsh on 2024-03-13 17:10_

---

_Label `bug` added by @charliermarsh on 2024-03-13 17:10_

---

_Label `windows` added by @charliermarsh on 2024-03-13 17:10_

---

_Marked ready for review by @charliermarsh on 2024-03-13 17:10_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:209 on 2024-03-13 17:10_

Important that this is `async` now because we don't want to block the thread during the backoff phase.

---

_@charliermarsh reviewed on 2024-03-13 17:10_

---

_@konstin approved on 2024-03-13 17:13_

---

_Merged by @charliermarsh on 2024-03-13 17:16_

---

_Closed by @charliermarsh on 2024-03-13 17:16_

---

_Branch deleted on 2024-03-13 17:16_

---
