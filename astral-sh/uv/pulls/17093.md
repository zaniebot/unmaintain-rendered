```yaml
number: 17093
title: Fix validate --project directory exists before use
type: pull_request
state: open
author: ddoemonn
labels:
  - breaking
assignees: []
base: main
head: fix-validate-project-flag
created_at: 2025-12-11T21:17:11Z
updated_at: 2025-12-12T18:56:53Z
url: https://github.com/astral-sh/uv/pull/17093
synced_at: 2026-01-10T05:49:14Z
```

# Fix validate --project directory exists before use

---

_Pull request opened by @ddoemonn on 2025-12-11 21:17_

## Summary

Fixes #17087

When `--project` is specified with a non-existent path, uv now fails immediately with a clear error message instead of silently continuing and potentially discovering a different project.

The fix validates that the provided `--project` path exists and is a directory before proceeding with any command execution.

## Test Plan

- Added two new unit tests: `run_project_not_found` and `run_project_is_file`
- Manually tested with non-existent directory, file path, and valid directory

---

_Label `breaking` added by @zanieb on 2025-12-12 18:56_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-12-12 18:56_

---

_Comment by @zanieb on 2025-12-12 18:56_

This looks good to me but is a breaking change.

It'd be good to add it as a warning first then make it fail in 0.10

---
