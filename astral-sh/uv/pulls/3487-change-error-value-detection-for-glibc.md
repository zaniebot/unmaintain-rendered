```yaml
number: 3487
title: Change error value detection for glibc
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/lib
created_at: 2024-05-09T15:15:07Z
updated_at: 2024-05-09T15:24:04Z
url: https://github.com/astral-sh/uv/pull/3487
synced_at: 2026-01-10T14:37:54Z
```

# Change error value detection for glibc

---

_Pull request opened by @charliermarsh on 2024-05-09 15:15_

## Summary

See: #3486. This just fixes the error message, not the underlying bug.


---

_Label `error messages` added by @charliermarsh on 2024-05-09 15:16_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/python/get_interpreter_info.py`:472 on 2024-05-09 15:16_

The return type of `_get_glibc_version` is `tuple[int, int]`, not an optional.

---

_@charliermarsh reviewed on 2024-05-09 15:16_

---

_@charliermarsh reviewed on 2024-05-09 15:16_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/python/get_interpreter_info.py`:472 on 2024-05-09 15:16_

(I don't want to change `_get_glibc_version` since it's vendored.)

---

_Comment by @ofek on 2024-05-09 15:20_

I don't know if this is helpful to you at all but in Hatch this is what we use to determine MUSL: https://github.com/pypa/hatch/blob/hatch-v1.10.0/src/hatch/python/resolve.py#L152

---

_Merged by @charliermarsh on 2024-05-09 15:24_

---

_Closed by @charliermarsh on 2024-05-09 15:24_

---

_Branch deleted on 2024-05-09 15:24_

---
