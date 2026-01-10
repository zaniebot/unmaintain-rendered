```yaml
number: 3465
title: "Ignore `compile_invalid_pyc_invalidation_mode` on macOS"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/byte
created_at: 2024-05-08T17:45:29Z
updated_at: 2024-05-08T18:17:04Z
url: https://github.com/astral-sh/uv/pull/3465
synced_at: 2026-01-10T14:37:54Z
```

# Ignore `compile_invalid_pyc_invalidation_mode` on macOS

---

_Pull request opened by @charliermarsh on 2024-05-08 17:45_

## Summary

This is annoying both locally in CI. If anyone wants to fuss with the filters to fix it, that's fine too, but IMO it's better to disable than leave it enabled on macOS for now.


---

_Label `testing` added by @charliermarsh on 2024-05-08 17:45_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-08 17:45_

---

_Review requested from @konstin by @charliermarsh on 2024-05-08 17:45_

---

_Comment by @konstin on 2024-05-08 17:55_

I've seen them fail on linux too recently, i unfortunately don't understand why

---

_@konstin approved on 2024-05-08 17:55_

---

_Merged by @charliermarsh on 2024-05-08 18:04_

---

_Closed by @charliermarsh on 2024-05-08 18:04_

---

_Branch deleted on 2024-05-08 18:04_

---

_Comment by @zanieb on 2024-05-08 18:17_

Yeah this is a constant annoyance. xref #2672 

---
