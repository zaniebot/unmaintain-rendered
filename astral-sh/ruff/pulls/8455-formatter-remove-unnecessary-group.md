```yaml
number: 8455
title: "Formatter: Remove unnecessary `group`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-unnecessary-group
created_at: 2023-11-03T02:32:01Z
updated_at: 2023-11-03T04:14:30Z
url: https://github.com/astral-sh/ruff/pull/8455
synced_at: 2026-01-12T15:55:26Z
```

# Formatter: Remove unnecessary `group`

---

_@MichaReiser_

## Summary

Small internal refactor that removes an unnecessary `group` around the default value.

## Test Plan

`cargo test`, similarity index remains unchanged (let's wait for the ecosystem check)


---

_Comment by @MichaReiser on 2023-11-03 02:32_

Current dependencies on/for this PR:
* main
  * **PR #8455** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8455" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8455?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-11-03 02:32_

---

_@MichaReiser reviewed on 2023-11-03 02:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2519 on 2023-11-03 02:32_

Okay, okay. And I deleted this unused code 😅 

---

_@charliermarsh reviewed on 2023-11-03 02:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2519 on 2023-11-03 02:33_

Impressive...

---

_@charliermarsh approved on 2023-11-03 02:33_

---

_Comment by @github-actions[bot] on 2023-11-03 02:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2023-11-03 04:14_

---

_Closed by @MichaReiser on 2023-11-03 04:14_

---

_Branch deleted on 2023-11-03 04:14_

---
