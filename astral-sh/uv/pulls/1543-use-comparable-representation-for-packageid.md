```yaml
number: 1543
title: "Use comparable representation for `PackageId`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pytz
created_at: 2024-02-16T21:09:25Z
updated_at: 2024-02-16T21:30:55Z
url: https://github.com/astral-sh/uv/pull/1543
synced_at: 2026-01-10T15:33:24Z
```

# Use comparable representation for `PackageId`

---

_Pull request opened by @charliermarsh on 2024-02-16 21:09_

## Summary

By using the display representation of `Version` to form a `PackageId`, we run the risk (as seen in the linked issue) of thinking that versions like `2021.1` and `2021.1.0` are not equivalent.

Closes https://github.com/astral-sh/uv/issues/1536


---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-16 21:09_

---

_Label `bug` added by @charliermarsh on 2024-02-16 21:09_

---

_@BurntSushi approved on 2024-02-16 21:27_

Sweet.

---

_Merged by @charliermarsh on 2024-02-16 21:30_

---

_Closed by @charliermarsh on 2024-02-16 21:30_

---

_Branch deleted on 2024-02-16 21:30_

---
