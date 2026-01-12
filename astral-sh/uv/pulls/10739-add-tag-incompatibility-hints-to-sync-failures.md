```yaml
number: 10739
title: Add tag incompatibility hints to sync failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/sync-error-hint
created_at: 2025-01-18T19:16:32Z
updated_at: 2025-01-20T17:46:48Z
url: https://github.com/astral-sh/uv/pull/10739
synced_at: 2026-01-12T16:09:27Z
```

# Add tag incompatibility hints to sync failures

---

_@charliermarsh_

## Summary

These are very similar to (and computed in the same way as) the hints we should during a failed resolution, but for install-time.

Closes #10635.

## Test Plan

As an example, when installing PyTorch on macOS with Python 3.13 (wheels exist for Linux):

```
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on macOS (`macosx_14_0_arm64`), but `torch` (v2.5.1) only has wheels for the following platform: `manylinux1_x86_64`
```


---

_Label `error messages` added by @charliermarsh on 2025-01-18 19:16_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-18 19:28_

---

_@zanieb approved on 2025-01-20 17:36_

---

_Merged by @charliermarsh on 2025-01-20 17:46_

---

_Closed by @charliermarsh on 2025-01-20 17:46_

---

_Branch deleted on 2025-01-20 17:46_

---
