```yaml
number: 15622
title: "Fix settings rendering for `extra-build-dependencies`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - documentation
assignees: []
merged: true
base: main
head: charlie/fix-settings
created_at: 2025-09-01T17:09:47Z
updated_at: 2025-09-02T13:06:24Z
url: https://github.com/astral-sh/uv/pull/15622
synced_at: 2026-01-10T06:44:33Z
```

# Fix settings rendering for `extra-build-dependencies`

---

_Pull request opened by @charliermarsh on 2025-09-01 17:09_

## Summary

This was fixed in https://github.com/astral-sh/uv/pull/15161, then reverted as it regressed the error handling. I've re-applied the change here, but moved the error handling to the runtime, rather than parse-time. I think this is slightly worse in that we no longer include the originating source code snippet, but it at least gives us the expected behavior :(

Closes https://github.com/astral-sh/uv/issues/15124.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-01 17:10_

---

_Review requested from @konstin by @charliermarsh on 2025-09-01 17:10_

---

_Label `bug` added by @charliermarsh on 2025-09-01 17:10_

---

_Marked ready for review by @charliermarsh on 2025-09-01 17:10_

---

_@konstin approved on 2025-09-02 09:56_

---

_Label `documentation` added by @konstin on 2025-09-02 09:56_

---

_@zanieb approved on 2025-09-02 12:58_

---

_Merged by @charliermarsh on 2025-09-02 13:06_

---

_Closed by @charliermarsh on 2025-09-02 13:06_

---

_Branch deleted on 2025-09-02 13:06_

---
