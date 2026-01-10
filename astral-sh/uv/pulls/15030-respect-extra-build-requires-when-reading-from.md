```yaml
number: 15030
title: Respect extra build requires when reading from wheel cache
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2025-08-02T18:11:34Z
updated_at: 2025-08-02T19:26:04Z
url: https://github.com/astral-sh/uv/pull/15030
synced_at: 2026-01-10T06:44:33Z
```

# Respect extra build requires when reading from wheel cache

---

_Pull request opened by @charliermarsh on 2025-08-02 18:11_

## Summary

We weren't including these in the cache key when constructing the install plan. We likely still read them from the cache later, but we may have reported the wrong number of prepares, etc.

---

_Label `bug` added by @charliermarsh on 2025-08-02 18:11_

---

_Marked ready for review by @charliermarsh on 2025-08-02 18:11_

---

_@charliermarsh reviewed on 2025-08-02 19:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:1674 on 2025-08-02 19:14_

On `main`, this shows `Prepared [N] packages in [TIME]` despite them all coming from the cache.

---

_Merged by @charliermarsh on 2025-08-02 19:26_

---

_Closed by @charliermarsh on 2025-08-02 19:26_

---

_Branch deleted on 2025-08-02 19:26_

---
