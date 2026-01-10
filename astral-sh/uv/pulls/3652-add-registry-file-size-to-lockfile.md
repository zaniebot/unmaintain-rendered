```yaml
number: 3652
title: Add registry file size to lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/file-size
created_at: 2024-05-18T17:19:19Z
updated_at: 2024-05-20T12:15:19Z
url: https://github.com/astral-sh/uv/pull/3652
synced_at: 2026-01-10T14:32:20Z
```

# Add registry file size to lockfile

---

_Pull request opened by @charliermarsh on 2024-05-18 17:19_

## Summary

Mentioned in #3611.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-18 17:19_

---

_Label `preview` added by @charliermarsh on 2024-05-18 17:19_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-18 17:19_

---

_@zanieb approved on 2024-05-18 17:20_

---

_Merged by @charliermarsh on 2024-05-19 02:27_

---

_Closed by @charliermarsh on 2024-05-19 02:27_

---

_Branch deleted on 2024-05-19 02:27_

---

_@BurntSushi reviewed on 2024-05-20 12:15_

Ah this is interesting. The size isn't required for lock semantics, but without it, I agree that we'd be leaving some perf on the table. Nice fix.

---
