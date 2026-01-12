```yaml
number: 4083
title: "Remove integration tests from `uv-resolver`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/res
created_at: 2024-06-06T01:27:09Z
updated_at: 2024-06-06T01:48:44Z
url: https://github.com/astral-sh/uv/pull/4083
synced_at: 2026-01-12T16:06:02Z
```

# Remove integration tests from `uv-resolver`

---

_@charliermarsh_

## Summary

I don't think it's worth maintaining this separate test harness for ~18 tests, when they can all be tested in the `uv` package itself. Let's drop the maintenance burden.


---

_Label `testing` added by @charliermarsh on 2024-06-06 01:27_

---

_Marked ready for review by @charliermarsh on 2024-06-06 01:27_

---

_Merged by @charliermarsh on 2024-06-06 01:48_

---

_Closed by @charliermarsh on 2024-06-06 01:48_

---

_Branch deleted on 2024-06-06 01:48_

---
