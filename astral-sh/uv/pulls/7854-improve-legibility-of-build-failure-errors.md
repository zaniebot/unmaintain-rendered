```yaml
number: 7854
title: Improve legibility of build failure errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2024-10-02T00:22:44Z
updated_at: 2024-10-03T12:52:54Z
url: https://github.com/astral-sh/uv/pull/7854
synced_at: 2026-01-12T16:08:02Z
```

# Improve legibility of build failure errors

---

_@charliermarsh_

## Summary

We now...

- Only show `stdout` and/or `stderr` if they're non-empty.
- Use a colorful header for the start of each section
- Highlight the step that failed

## Test Plan

![Screenshot 2024-10-01 at 8 18 42 PM](https://github.com/user-attachments/assets/c3dadf6d-11be-468d-940c-a0a29c4a88f0)


---

_Label `error messages` added by @charliermarsh on 2024-10-02 00:22_

---

_Marked ready for review by @charliermarsh on 2024-10-02 00:22_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-02 02:18_

---

_Review requested from @konstin by @charliermarsh on 2024-10-02 02:18_

---

_Review comment by @konstin on `crates/uv-build-frontend/src/error.rs`:83 on 2024-10-02 10:20_

Can we use a wrapper type with `stdout: Option<String>, stderr: Option<String>` that `impl Display` instead of duplicating this?

---

_@konstin approved on 2024-10-02 10:20_

---

_@zanieb approved on 2024-10-02 14:43_

---

_Merged by @charliermarsh on 2024-10-03 12:52_

---

_Closed by @charliermarsh on 2024-10-03 12:52_

---

_Branch deleted on 2024-10-03 12:52_

---
