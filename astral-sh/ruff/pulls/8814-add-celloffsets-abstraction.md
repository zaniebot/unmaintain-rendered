```yaml
number: 8814
title: "Add `CellOffsets` abstraction"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/cell-offsets
created_at: 2023-11-21T22:19:12Z
updated_at: 2023-11-22T15:34:18Z
url: https://github.com/astral-sh/ruff/pull/8814
synced_at: 2026-01-10T23:40:55Z
```

# Add `CellOffsets` abstraction

---

_Pull request opened by @dhruvmanila on 2023-11-21 22:19_

Refactor `Notebook::cell_offsets` to use an abstract struct for storing the cell
offsets. This will allow us to add useful methods on it.


---

_Comment by @dhruvmanila on 2023-11-21 22:19_

Current dependencies on/for this PR:
* main
  * **PR #8813** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8813?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8814** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8814?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #8815** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8815?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8814?utm_source=stack-comment).

---

_Label `internal` added by @dhruvmanila on 2023-11-21 22:19_

---

_@charliermarsh approved on 2023-11-21 23:46_

Nice, great idea.

---

_Comment by @github-actions[bot] on 2023-11-21 23:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dhruvmanila on 2023-11-22 15:27_

---

_Closed by @dhruvmanila on 2023-11-22 15:27_

---

_Branch deleted on 2023-11-22 15:27_

---
