```yaml
number: 15788
title: Add conflict detection between --only-group and --extra flags
type: pull_request
state: merged
author: harshithvh
labels: []
assignees: []
merged: true
base: main
head: hvh/only-group-extra-conflict
created_at: 2025-09-11T13:31:47Z
updated_at: 2025-09-11T15:34:50Z
url: https://github.com/astral-sh/uv/pull/15788
synced_at: 2026-01-12T16:11:56Z
```

# Add conflict detection between --only-group and --extra flags

---

_@harshithvh_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

- Added `conflicts_with = "only_group"` to `--extra` arguments in `SyncArgs`, `RunArgs`, and `ExportArgs`
- Added tests to verify proper conflict detection and error messages

**Before:** The `--extra` flag was silently ignored when used with `--only-group`
**After:** Clear error message: `error: the argument '--only-group <ONLY_GROUP>' cannot be used with '--extra <EXTRA>'`

fixes: #15676 

## Test Plan

- Tests confirm proper error message format when `--only-group` and `--extra` are used together
- Verified existing functionality remains unchanged when flags are used independently


---

_@zanieb approved on 2025-09-11 14:25_

Thanks! We should have `--all-extras` conflict with `--only-group` too

---

_Merged by @zanieb on 2025-09-11 15:34_

---

_Closed by @zanieb on 2025-09-11 15:34_

---
