```yaml
number: 12396
title: "Ignore `--find-links` entries for pinned indexes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pin
created_at: 2025-03-22T19:21:59Z
updated_at: 2025-03-23T12:46:38Z
url: https://github.com/astral-sh/uv/pull/12396
synced_at: 2026-01-12T16:10:15Z
```

# Ignore `--find-links` entries for pinned indexes

---

_@charliermarsh_

## Summary

In general, we merge `--find-links` entries into each index. If a package is pinned to an index, though, it seems surprising (and wrong) that we'd ever select a distribution from `--find-links`. This PR modifies the provider to ignore `--find-links` for any explicitly pinned packages.


---

_Review requested from @zanieb by @charliermarsh on 2025-03-22 19:22_

---

_Review requested from @konstin by @charliermarsh on 2025-03-22 19:22_

---

_Label `bug` added by @charliermarsh on 2025-03-22 19:22_

---

_Comment by @charliermarsh on 2025-03-22 19:22_

This is arguably breaking, though I think it's fair to consider this a bug.

---

_@zanieb approved on 2025-03-23 03:07_

---

_Merged by @charliermarsh on 2025-03-23 12:46_

---

_Closed by @charliermarsh on 2025-03-23 12:46_

---

_Branch deleted on 2025-03-23 12:46_

---
