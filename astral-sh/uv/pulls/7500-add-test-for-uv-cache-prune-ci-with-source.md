```yaml
number: 7500
title: "Add test for `uv cache prune --ci` with source distributions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-09-18T13:36:47Z
updated_at: 2024-09-18T14:01:08Z
url: https://github.com/astral-sh/uv/pull/7500
synced_at: 2026-01-12T16:07:51Z
```

# Add test for `uv cache prune --ci` with source distributions

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/issues/7485. The test was using `uv pip sync` which doesn't require fetching metadata, and the failure was in fetching metadata.

---

_Label `testing` added by @charliermarsh on 2024-09-18 13:36_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-18 13:37_

---

_@zanieb approved on 2024-09-18 13:56_

---

_Merged by @charliermarsh on 2024-09-18 14:01_

---

_Closed by @charliermarsh on 2024-09-18 14:01_

---

_Branch deleted on 2024-09-18 14:01_

---
