```yaml
number: 2258
title: "Add support for `--no-build-isolation`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/build-isolation
created_at: 2024-03-07T02:19:17Z
updated_at: 2024-03-07T14:04:03Z
url: https://github.com/astral-sh/uv/pull/2258
synced_at: 2026-01-10T14:54:43Z
```

# Add support for `--no-build-isolation`

---

_Pull request opened by @charliermarsh on 2024-03-07 02:19_

## Summary

This PR adds support for pip's `--no-build-isolation`. When enabled, build requirements won't be installed during PEP 517-style builds, but the source environment _will_ be used when executing the build steps themselves.

Closes https://github.com/astral-sh/uv/issues/1715.


---

_Label `enhancement` added by @charliermarsh on 2024-03-07 02:19_

---

_Label `compatibility` added by @charliermarsh on 2024-03-07 02:19_

---

_Marked ready for review by @charliermarsh on 2024-03-07 02:25_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-07 02:25_

---

_Review requested from @konstin by @charliermarsh on 2024-03-07 02:25_

---

_@konstin approved on 2024-03-07 09:01_

---

_Merged by @charliermarsh on 2024-03-07 14:04_

---

_Closed by @charliermarsh on 2024-03-07 14:04_

---

_Branch deleted on 2024-03-07 14:04_

---
