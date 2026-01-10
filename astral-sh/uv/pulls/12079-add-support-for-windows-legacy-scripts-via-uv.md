```yaml
number: 12079
title: Add support for Windows legacy scripts via uv tool run
type: pull_request
state: merged
author: samypr100
labels:
  - windows
assignees: []
merged: true
base: main
head: uvx-legacy-windows-scripts-support
created_at: 2025-03-10T02:49:49Z
updated_at: 2025-03-11T16:22:59Z
url: https://github.com/astral-sh/uv/pull/12079
synced_at: 2026-01-10T11:10:39Z
```

# Add support for Windows legacy scripts via uv tool run

---

_Pull request opened by @samypr100 on 2025-03-10 02:49_

## Summary

Follow up to https://github.com/astral-sh/uv/pull/11888 with added support for uv tool run.

Changes
* Added functionality for running windows scripts in previous PR was moved from run.rs to uv_shell::runnable.
* EXE was added as a supported type, this simplified integration across both uv run and uvx while retaining a backwards compatible behavior and properly prioritizing .exe over others. Name was adjusted to runnable as a result to better represent intent.

## Test Plan

New tests added.

## Documentation

Added new documentation.

---

_Label `windows` added by @samypr100 on 2025-03-10 02:49_

---

_Marked ready for review by @samypr100 on 2025-03-10 11:24_

---

_@zanieb approved on 2025-03-11 14:02_

Great work :)

---

_Merged by @zanieb on 2025-03-11 14:02_

---

_Closed by @zanieb on 2025-03-11 14:02_

---

_Branch deleted on 2025-03-11 16:22_

---
