```yaml
number: 3221
title: "chore: use uv-version in uv-virtualenv"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: uv-venv-small-cleanup
created_at: 2024-04-23T20:05:40Z
updated_at: 2024-05-03T20:10:07Z
url: https://github.com/astral-sh/uv/pull/3221
synced_at: 2026-01-12T16:05:30Z
```

# chore: use uv-version in uv-virtualenv

---

_@samypr100_

## Summary

This is mainly a cleanup PR to leverage uv-version in uv-virtualenv instead of passing it via `uv`.
In #1852 I introduced the ability to pass extra cfg to `gourgeist` for the primary purpose of passing the uv version, but since the dawn of the uv-version crate dynamically passing more values to pyvenv.cfg is no longer needed.

## Test Plan

Existing `uv` tests should still verify `uv = <version>` exists in the venv and make sure no regressions were introduced.


---

_Review requested from @konstin by @zanieb on 2024-04-23 20:06_

---

_Comment by @zanieb on 2024-04-23 20:06_

Cool thanks!

---

_Marked ready for review by @samypr100 on 2024-04-23 20:16_

---

_Comment by @charliermarsh on 2024-04-23 20:18_

Nice.

---

_Label `internal` added by @charliermarsh on 2024-04-23 20:18_

---

_@charliermarsh approved on 2024-04-23 20:18_

---

_Merged by @charliermarsh on 2024-04-23 20:18_

---

_Closed by @charliermarsh on 2024-04-23 20:18_

---

_Branch deleted on 2024-05-03 20:10_

---
