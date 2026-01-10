```yaml
number: 7822
title: "Use `serde-untagged` to improve some untagged enum error messages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/serde-untagged
created_at: 2024-09-30T23:32:49Z
updated_at: 2024-09-30T23:40:22Z
url: https://github.com/astral-sh/uv/pull/7822
synced_at: 2026-01-10T12:53:56Z
```

# Use `serde-untagged` to improve some untagged enum error messages

---

_Pull request opened by @charliermarsh on 2024-09-30 23:32_

## Summary

This is related to https://github.com/astral-sh/uv/issues/7817, but doesn't close it.

---

_Label `error messages` added by @charliermarsh on 2024-09-30 23:32_

---

_@charliermarsh reviewed on 2024-09-30 23:36_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:247 on 2024-09-30 23:36_

This is the payoff. Without this change, you just get: `data did not match any variant of untagged enum SourcesWire`

---

_Marked ready for review by @charliermarsh on 2024-09-30 23:36_

---

_Merged by @charliermarsh on 2024-09-30 23:40_

---

_Closed by @charliermarsh on 2024-09-30 23:40_

---

_Branch deleted on 2024-09-30 23:40_

---
