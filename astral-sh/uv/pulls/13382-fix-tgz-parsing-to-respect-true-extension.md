```yaml
number: 13382
title: "Fix `.tgz` parsing to respect true extension"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tlz
created_at: 2025-05-10T20:37:47Z
updated_at: 2025-05-19T06:03:37Z
url: https://github.com/astral-sh/uv/pull/13382
synced_at: 2026-01-10T11:10:41Z
```

# Fix `.tgz` parsing to respect true extension

---

_Pull request opened by @charliermarsh on 2025-05-10 20:37_

## Summary

We mapped both `.tgz` and `.tar.gz` to the same enum variant; later, though, we made the assumption that a file marked with that variant ended with exactly `.tar.gz`. Instead, we need to preserve the originating suffix.

Closes https://github.com/astral-sh/uv/issues/13372.


---

_Label `bug` added by @charliermarsh on 2025-05-10 20:37_

---

_Marked ready for review by @charliermarsh on 2025-05-10 20:38_

---

_Closed by @charliermarsh on 2025-05-10 20:45_

---

_Reopened by @charliermarsh on 2025-05-10 20:45_

---

_Merged by @charliermarsh on 2025-05-10 20:55_

---

_Closed by @charliermarsh on 2025-05-10 20:55_

---

_Branch deleted on 2025-05-10 20:55_

---
