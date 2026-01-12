```yaml
number: 15300
title: "fix: Handle dotted package names in script path resolution"
type: pull_request
state: merged
author: crh23
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/windows-executable-dotted-names
created_at: 2025-08-15T10:07:17Z
updated_at: 2025-08-15T21:44:59Z
url: https://github.com/astral-sh/uv/pull/15300
synced_at: 2026-01-12T16:11:40Z
```

# fix: Handle dotted package names in script path resolution

---

_@crh23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix WindowsRunnable::from_script_path to correctly append extensions instead of replacing them when resolving executable paths. This resolves https://github.com/astral-sh/uv/issues/15165#issue-3304086689.

- Add add_extension_to_path helper that appends extensions properly
- Update extension resolution to use the new helper
- Add tests

## Test Plan

Added unit tests for the new and existing functionality that the change touches. Tested manually locally on Windows.
<!-- How was it tested? -->


---

_Comment by @zanieb on 2025-08-15 15:52_

Could you add an integration test in `it/tool_run.rs`? e.g., by creating a package in `scripts/packages` then using `uv tool run --from <workspace>/scripts/packages/<name> <name>`?

---

_Marked ready for review by @zanieb on 2025-08-15 20:56_

---

_Label `bug` added by @zanieb on 2025-08-15 20:57_

---

_@zanieb approved on 2025-08-15 20:57_

Thanks! I made some stylistic tweaks.

---

_Merged by @zanieb on 2025-08-15 21:44_

---

_Closed by @zanieb on 2025-08-15 21:44_

---
