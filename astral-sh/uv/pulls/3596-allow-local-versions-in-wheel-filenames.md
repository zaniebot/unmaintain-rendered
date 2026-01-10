```yaml
number: 3596
title: Allow local versions in wheel filenames
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/local
created_at: 2024-05-14T23:48:45Z
updated_at: 2024-05-15T00:02:10Z
url: https://github.com/astral-sh/uv/pull/3596
synced_at: 2026-01-10T14:37:54Z
```

# Allow local versions in wheel filenames

---

_Pull request opened by @charliermarsh on 2024-05-14 23:48_

## Summary

Closes https://github.com/astral-sh/uv/issues/3594.

## Test Plan

`cargo run pip install --verbose https://github.com/Dao-AILab/flash-attention/releases/download/v2.5.8/flash_attn-2.5.8+cu122torch2.3cxx11abiFALSE-cp310-cp310-linux_x86_64.whl --no-deps`


---

_Label `compatibility` added by @charliermarsh on 2024-05-14 23:48_

---

_Marked ready for review by @charliermarsh on 2024-05-14 23:48_

---

_Merged by @charliermarsh on 2024-05-15 00:02_

---

_Closed by @charliermarsh on 2024-05-15 00:02_

---

_Branch deleted on 2024-05-15 00:02_

---
