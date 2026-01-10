```yaml
number: 6815
title: "Hint at `--no-workspace` in `uv init` failures"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/i
created_at: 2024-08-29T14:45:58Z
updated_at: 2024-08-29T16:57:10Z
url: https://github.com/astral-sh/uv/pull/6815
synced_at: 2026-01-10T12:53:35Z
```

# Hint at `--no-workspace` in `uv init` failures

---

_Pull request opened by @charliermarsh on 2024-08-29 14:45_

## Summary

We now both (1) include the `pyproject.toml` (which we were doing sometimes, but inconsistently) and (2) hint at `--no-workspace`).

Closes https://github.com/astral-sh/uv/issues/6393.

## Test Plan

Looks like this now:

![Screenshot 2024-08-29 at 10 44 55 AM](https://github.com/user-attachments/assets/a7c4cbff-704b-4dac-b0e4-e8e12a2b1f5d)


---

_Label `error messages` added by @charliermarsh on 2024-08-29 14:46_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-29 14:46_

---

_Review requested from @konstin by @charliermarsh on 2024-08-29 14:46_

---

_Marked ready for review by @charliermarsh on 2024-08-29 14:46_

---

_@konstin approved on 2024-08-29 14:46_

---

_@zanieb approved on 2024-08-29 16:54_

---

_Merged by @charliermarsh on 2024-08-29 16:57_

---

_Closed by @charliermarsh on 2024-08-29 16:57_

---

_Branch deleted on 2024-08-29 16:57_

---
