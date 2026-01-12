```yaml
number: 12504
title: "Enable necessary optional features in `uv-settings`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - no-build
assignees: []
merged: true
base: main
head: charlie/feat
created_at: 2025-03-27T02:21:41Z
updated_at: 2025-03-27T02:30:01Z
url: https://github.com/astral-sh/uv/pull/12504
synced_at: 2026-01-12T16:10:18Z
```

# Enable necessary optional features in `uv-settings`

---

_@charliermarsh_

## Summary

We tend not to run tests for individual crates, which can lead to weird situations like this, where crates are missing optional features that are otherwise installed globally.

## Test Plan

Run `cargo test --profile fast-build -p uv-scripts`, which otherwise fails to compile.

---

_Label `internal` added by @charliermarsh on 2025-03-27 02:21_

---

_Label `no-build` added by @charliermarsh on 2025-03-27 02:21_

---

_Review requested from @konstin by @charliermarsh on 2025-03-27 02:22_

---

_Marked ready for review by @charliermarsh on 2025-03-27 02:22_

---

_Merged by @charliermarsh on 2025-03-27 02:30_

---

_Closed by @charliermarsh on 2025-03-27 02:30_

---

_Branch deleted on 2025-03-27 02:30_

---
