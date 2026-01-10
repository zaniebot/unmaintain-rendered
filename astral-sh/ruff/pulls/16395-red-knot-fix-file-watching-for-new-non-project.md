```yaml
number: 16395
title: "[red-knot] Fix file watching for new non-project files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-non-project-files
created_at: 2025-02-26T13:47:43Z
updated_at: 2025-02-26T15:10:15Z
url: https://github.com/astral-sh/ruff/pull/16395
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Fix file watching for new non-project files

---

_Pull request opened by @MichaReiser on 2025-02-26 13:47_

## Summary

This PR fixes a bug in Red Knot's file watching where a new
non-project file (a file created outside the project's root) was
incorrectly considered as being part of the project. 

Testing whether the new file's path starts with the project's root path is sufficient because neither Ruff nor Red Knot follow symlinks when traversing
the project's directory tree.

## Test Plan

Added file watching test


---

_Review requested from @carljm by @MichaReiser on 2025-02-26 13:47_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-26 13:47_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-26 13:47_

---

_Label `red-knot` added by @MichaReiser on 2025-02-26 13:47_

---

_@carljm approved on 2025-02-26 15:04_

Makes sense!

---

_Merged by @MichaReiser on 2025-02-26 15:10_

---

_Closed by @MichaReiser on 2025-02-26 15:10_

---

_Branch deleted on 2025-02-26 15:10_

---
