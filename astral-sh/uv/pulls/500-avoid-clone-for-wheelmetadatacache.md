```yaml
number: 500
title: "Avoid clone for `WheelMetadataCache`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lifetime
created_at: 2023-11-24T18:21:01Z
updated_at: 2023-11-25T23:34:01Z
url: https://github.com/astral-sh/uv/pull/500
synced_at: 2026-01-12T16:03:59Z
```

# Avoid clone for `WheelMetadataCache`

---

_@charliermarsh_

This doesn't need to own the underlying data which allows us to remove a number of clones.

---

_Review requested from @konstin by @charliermarsh on 2023-11-24 18:21_

---

_Label `internal` added by @charliermarsh on 2023-11-24 18:21_

---

_@konstin approved on 2023-11-24 20:10_

---

_Comment by @konstin on 2023-11-24 20:11_

Is the test failure some url normalization thing?

---

_Comment by @charliermarsh on 2023-11-24 22:24_

It’s a failure in the upstream PR, logic error on my part.

---

_Merged by @charliermarsh on 2023-11-25 23:34_

---

_Closed by @charliermarsh on 2023-11-25 23:34_

---

_Branch deleted on 2023-11-25 23:34_

---
