```yaml
number: 8564
title: "Support cyclic dependencies in `uv tree`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cy
created_at: 2024-10-25T13:27:47Z
updated_at: 2024-10-26T17:30:44Z
url: https://github.com/astral-sh/uv/pull/8564
synced_at: 2026-01-10T12:54:12Z
```

# Support cyclic dependencies in `uv tree`

---

_Pull request opened by @charliermarsh on 2024-10-25 13:27_

## Summary

Closes https://github.com/astral-sh/uv/issues/8551.


---

_Label `bug` added by @charliermarsh on 2024-10-25 13:27_

---

_Comment by @charliermarsh on 2024-10-25 14:17_

Okay this isn't quite right -- it doesn't handle inversions correctly.

---

_Comment by @charliermarsh on 2024-10-25 15:15_

I have a version that _does_ handle cycles via SCCs but it doesn't preserve alphabetical ordering of package names.

---

_Merged by @charliermarsh on 2024-10-26 17:30_

---

_Closed by @charliermarsh on 2024-10-26 17:30_

---

_Branch deleted on 2024-10-26 17:30_

---
