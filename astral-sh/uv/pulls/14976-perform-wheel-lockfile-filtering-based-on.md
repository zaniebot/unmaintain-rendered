```yaml
number: 14976
title: Perform wheel lockfile filtering based on platform and OS intersection
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/intersection
created_at: 2025-07-30T14:48:50Z
updated_at: 2025-07-30T15:12:24Z
url: https://github.com/astral-sh/uv/pull/14976
synced_at: 2026-01-12T16:11:31Z
```

# Perform wheel lockfile filtering based on platform and OS intersection

---

_@charliermarsh_

## Summary

Ensures that if the user filters to macOS ARM, we don't include macOS x86_64 wheels.

Closes https://github.com/astral-sh/uv/issues/14901.


---

_Review requested from @konstin by @charliermarsh on 2025-07-30 14:48_

---

_Label `enhancement` added by @charliermarsh on 2025-07-30 14:48_

---

_Marked ready for review by @charliermarsh on 2025-07-30 14:48_

---

_@zanieb approved on 2025-07-30 14:51_

---

_Merged by @charliermarsh on 2025-07-30 15:12_

---

_Closed by @charliermarsh on 2025-07-30 15:12_

---

_Branch deleted on 2025-07-30 15:12_

---
