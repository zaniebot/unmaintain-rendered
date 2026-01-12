```yaml
number: 11572
title: "Make `ruff_notebook` a workspace dependency in `ruff_server`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: server-notebook-workspace-dependency
created_at: 2024-05-27T16:32:06Z
updated_at: 2024-05-28T07:26:40Z
url: https://github.com/astral-sh/ruff/pull/11572
synced_at: 2026-01-12T15:55:38Z
```

# Make `ruff_notebook` a workspace dependency in `ruff_server`

---

_@MichaReiser_

## Summary
The `ruff_notebook` dependency was added in https://github.dev/astral-sh/ruff/pull/11206

I suspect it's a merge conflict. So lets update the dependency to use the format that Charlie prefers ;)

## Test Plan

`cargo build`


---

_@charliermarsh approved on 2024-05-27 16:32_

Yup :)

---

_Label `internal` added by @MichaReiser on 2024-05-27 16:36_

---

_Comment by @github-actions[bot] on 2024-05-27 16:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-05-28 07:26_

---

_Closed by @MichaReiser on 2024-05-28 07:26_

---

_Branch deleted on 2024-05-28 07:26_

---
