```yaml
number: 16655
title: "Remove `torch-model-archiver` and `torch-tb-profiler` from PyTorch backend"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tb
created_at: 2025-11-09T19:03:36Z
updated_at: 2025-11-10T15:26:14Z
url: https://github.com/astral-sh/uv/pull/16655
synced_at: 2026-01-10T06:28:12Z
```

# Remove `torch-model-archiver` and `torch-tb-profiler` from PyTorch backend

---

_Pull request opened by @charliermarsh on 2025-11-09 19:03_

## Summary

These are present on the PyTorch index, but only at very old versions. The PyPI versions are newer, and seemingly these don't need to be built against CUDA, etc.

Closes https://github.com/astral-sh/uv/issues/16651.


---

_Label `bug` added by @charliermarsh on 2025-11-09 19:03_

---

_Review requested from @zanieb by @charliermarsh on 2025-11-09 19:03_

---

_Review requested from @konstin by @charliermarsh on 2025-11-09 19:03_

---

_Marked ready for review by @charliermarsh on 2025-11-09 19:03_

---

_@konstin approved on 2025-11-10 10:39_

---

_Merged by @charliermarsh on 2025-11-10 15:26_

---

_Closed by @charliermarsh on 2025-11-10 15:26_

---

_Branch deleted on 2025-11-10 15:26_

---
