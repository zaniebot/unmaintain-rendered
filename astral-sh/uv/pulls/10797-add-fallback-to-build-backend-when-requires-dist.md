```yaml
number: 10797
title: "Add fallback to build backend when `Requires-Dist` mismatches"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/fb
created_at: 2025-01-21T00:18:58Z
updated_at: 2025-01-21T00:45:57Z
url: https://github.com/astral-sh/uv/pull/10797
synced_at: 2026-01-10T11:45:11Z
```

# Add fallback to build backend when `Requires-Dist` mismatches

---

_Pull request opened by @charliermarsh on 2025-01-21 00:18_

## Summary

This is a smaller alternative to #10794. If the `Requires-Dist` that we extract statically doesn't match the lockfile metadata, we now go back to the distribution database to double-check. Checking the `Requires-Dist` is itself very cheap, so in the worst case, we're just paying the same cost as prior to this optimization.

Closes https://github.com/astral-sh/uv/issues/10776.


---

_Label `bug` added by @charliermarsh on 2025-01-21 00:21_

---

_Merged by @charliermarsh on 2025-01-21 00:45_

---

_Closed by @charliermarsh on 2025-01-21 00:45_

---

_Branch deleted on 2025-01-21 00:45_

---
