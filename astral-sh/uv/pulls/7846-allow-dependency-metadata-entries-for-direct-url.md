```yaml
number: 7846
title: Allow dependency metadata entries for direct URL requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/dep-meta
created_at: 2024-10-01T15:35:14Z
updated_at: 2024-10-23T02:18:51Z
url: https://github.com/astral-sh/uv/pull/7846
synced_at: 2026-01-10T12:53:57Z
```

# Allow dependency metadata entries for direct URL requirements

---

_Pull request opened by @charliermarsh on 2024-10-01 15:35_

## Summary

This is part of making https://github.com/astral-sh/uv/issues/7299#issuecomment-2385286341 better. You can now use `tool.uv.dependency-metadata` for direct URL requirements. Unfortunately, you _must_ include a version, since we need one to perform resolution.

For example:
```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["flash-attn"]

[tool.uv.sources]
flash-attn = { git = "https://github.com/Dao-AILab/flash-attention", tag = "v2.6.3" }

[[tool.uv.dependency-metadata]]
name = "flash-attn"
version = "2.6.3"
requires-dist = ["torch", "einops"]
```


---

_Review requested from @zanieb by @charliermarsh on 2024-10-01 15:35_

---

_Review requested from @konstin by @charliermarsh on 2024-10-01 15:35_

---

_Label `enhancement` added by @charliermarsh on 2024-10-01 15:35_

---

_Marked ready for review by @charliermarsh on 2024-10-01 15:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:13032 on 2024-10-01 15:54_

We should validate that this is correct...

---

_@charliermarsh reviewed on 2024-10-01 15:54_

---

_Comment by @charliermarsh on 2024-10-01 16:06_

This panics for Git dependencies that aren't pinned to specific commits, will fix.

---

_Converted to draft by @charliermarsh on 2024-10-01 16:14_

---

_Renamed from "Allow dependency metadata entires for direct URL requirements" to "Allow dependency metadata entries for direct URL requirements" by @charliermarsh on 2024-10-02 09:22_

---

_Marked ready for review by @charliermarsh on 2024-10-23 01:51_

---

_Merged by @charliermarsh on 2024-10-23 02:01_

---

_Closed by @charliermarsh on 2024-10-23 02:01_

---

_Branch deleted on 2024-10-23 02:01_

---
