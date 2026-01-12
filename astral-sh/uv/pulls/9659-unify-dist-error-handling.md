```yaml
number: 9659
title: Unify dist error handling
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/unify-dist-err
created_at: 2024-12-05T12:44:42Z
updated_at: 2024-12-06T01:54:16Z
url: https://github.com/astral-sh/uv/pull/9659
synced_at: 2026-01-12T16:08:55Z
```

# Unify dist error handling

---

_@konstin_

This came up when trying to improve the build error reporting. Introduces `DistErrorKind` to avoid error variants for each case that are only different in one line of the message.

---

_Label `internal` added by @konstin on 2024-12-05 12:44_

---

_@charliermarsh approved on 2024-12-05 17:19_

---

_Merged by @charliermarsh on 2024-12-06 01:54_

---

_Closed by @charliermarsh on 2024-12-06 01:54_

---

_Branch deleted on 2024-12-06 01:54_

---
