```yaml
number: 4257
title: Move concurrency settings to top-level
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - breaking
assignees: []
merged: true
base: release/3
head: charlie/context
created_at: 2024-06-11T23:55:51Z
updated_at: 2024-08-19T19:37:00Z
url: https://github.com/astral-sh/uv/pull/4257
synced_at: 2026-01-12T16:06:07Z
```

# Move concurrency settings to top-level

---

_@charliermarsh_

## Summary

These are global and non-specific to the `pip` API, so I think they should be elevated.

## Test Plan

- Ran `UV_CONCURRENT_DOWNLOADS=1 cargo run pip list`; verified that `downloads` resolved to 1.
- Added `concurrent-downloads = 5` under `[tool.uv]` in `pyproject.toml`; ran `cargo run pip list`; verified that `downloads` resolved to 5.
- Ran `UV_CONCURRENT_DOWNLOADS=1 cargo run pip list`; verified that `downloads` resolved to 1.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-11 23:55_

---

_Label `configuration` added by @charliermarsh on 2024-06-11 23:55_

---

_Label `breaking` added by @charliermarsh on 2024-06-11 23:55_

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-06-11 23:56_

---

_@ibraheemdev approved on 2024-06-14 10:46_

---

_Merged by @zanieb on 2024-08-19 19:36_

---

_Closed by @zanieb on 2024-08-19 19:36_

---

_Branch deleted on 2024-08-19 19:37_

---
