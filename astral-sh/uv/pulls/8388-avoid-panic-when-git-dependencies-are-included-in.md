```yaml
number: 8388
title: Avoid panic when Git dependencies are included in fork markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prec
created_at: 2024-10-20T17:44:31Z
updated_at: 2024-10-20T18:42:22Z
url: https://github.com/astral-sh/uv/pull/8388
synced_at: 2026-01-10T12:54:08Z
```

# Avoid panic when Git dependencies are included in fork markers

---

_Pull request opened by @charliermarsh on 2024-10-20 17:44_

## Summary

Rather than relying on the distribution and package URL being the same (which isn't true for Git dependencies), we can just use the intersection of the markers directly.

Closes https://github.com/astral-sh/uv/issues/8381.


---

_Label `bug` added by @charliermarsh on 2024-10-20 17:44_

---

_Merged by @charliermarsh on 2024-10-20 18:42_

---

_Closed by @charliermarsh on 2024-10-20 18:42_

---

_Branch deleted on 2024-10-20 18:42_

---
