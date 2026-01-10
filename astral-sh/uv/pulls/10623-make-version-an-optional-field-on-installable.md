```yaml
number: 10623
title: "Make `version` an optional field on installable distribution type"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2025-01-15T03:02:41Z
updated_at: 2025-01-15T16:31:42Z
url: https://github.com/astral-sh/uv/pull/10623
synced_at: 2026-01-10T11:44:59Z
```

# Make `version` an optional field on installable distribution type

---

_Pull request opened by @charliermarsh on 2025-01-15 03:02_

## Summary

I previously made this required, but we now need to be able to create these from a lockfile that _omits_ versions for dynamic source trees. They should still be present in most cases, but it's best-effort.


---

_Label `internal` added by @charliermarsh on 2025-01-15 03:02_

---

_Marked ready for review by @charliermarsh on 2025-01-15 03:02_

---

_@konstin approved on 2025-01-15 07:58_

---

_Merged by @charliermarsh on 2025-01-15 16:31_

---

_Closed by @charliermarsh on 2025-01-15 16:31_

---

_Branch deleted on 2025-01-15 16:31_

---
