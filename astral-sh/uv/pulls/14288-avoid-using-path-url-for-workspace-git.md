```yaml
number: 14288
title: "Avoid using path URL for workspace Git dependencies in `requirements.txt`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ver
created_at: 2025-06-26T17:54:48Z
updated_at: 2025-06-26T19:48:13Z
url: https://github.com/astral-sh/uv/pull/14288
synced_at: 2026-01-10T11:10:43Z
```

# Avoid using path URL for workspace Git dependencies in `requirements.txt`

---

_Pull request opened by @charliermarsh on 2025-06-26 17:54_

## Summary

Closes https://github.com/astral-sh/uv/issues/13020.


---

_Renamed from "Avoid using path URL for workspace Git dependencies in requirements.txt" to "Avoid using path URL for workspace Git dependencies in `requirements.txt`" by @charliermarsh on 2025-06-26 17:54_

---

_Review requested from @konstin by @charliermarsh on 2025-06-26 17:54_

---

_Label `bug` added by @charliermarsh on 2025-06-26 17:54_

---

_Comment by @charliermarsh on 2025-06-26 17:55_

Need to figure out a test, but it does resolve the linked issue.

---

_@konstin reviewed on 2025-06-26 18:55_

Fix looks good, but we need the test

---

_Comment by @charliermarsh on 2025-06-26 19:09_

@konstin -- Added.

---

_Review comment by @konstin on `crates/uv-redacted/src/lib.rs`:180 on 2025-06-26 19:13_

iirc this was cause ssh needs a username while https doesn't, but it doesn't hurt either.

---

_@konstin reviewed on 2025-06-26 19:13_

---

_@konstin approved on 2025-06-26 19:13_

---

_Merged by @charliermarsh on 2025-06-26 19:48_

---

_Closed by @charliermarsh on 2025-06-26 19:48_

---

_Branch deleted on 2025-06-26 19:48_

---
