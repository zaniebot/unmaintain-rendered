```yaml
number: 16045
title: Re-order lock validation checks by severity
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comp
created_at: 2025-09-27T18:42:02Z
updated_at: 2025-09-29T13:42:22Z
url: https://github.com/astral-sh/uv/pull/16045
synced_at: 2026-01-10T06:36:15Z
```

# Re-order lock validation checks by severity

---

_Pull request opened by @charliermarsh on 2025-09-27 18:42_

## Summary

I don't think it's common for this to matter, but in theory at least it's important that these are ordered by severity. Otherwise, e.g, changing the pre-release mode (and then returning early) could mean we retain the forks when we otherwise shouldn't.


---

_Label `bug` added by @charliermarsh on 2025-09-27 18:42_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-27 18:42_

---

_Review requested from @konstin by @charliermarsh on 2025-09-27 18:42_

---

_Marked ready for review by @charliermarsh on 2025-09-27 18:42_

---

_@konstin approved on 2025-09-29 08:29_

---

_@zanieb approved on 2025-09-29 13:42_

---

_Merged by @charliermarsh on 2025-09-29 13:42_

---

_Closed by @charliermarsh on 2025-09-29 13:42_

---

_Branch deleted on 2025-09-29 13:42_

---
