```yaml
number: 9111
title: "Add `as_slice` method for all string nodes"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/as-slice
created_at: 2023-12-13T03:00:54Z
updated_at: 2023-12-13T06:41:35Z
url: https://github.com/astral-sh/ruff/pull/9111
synced_at: 2026-01-10T23:31:11Z
```

# Add `as_slice` method for all string nodes

---

_Pull request opened by @dhruvmanila on 2023-12-13 03:00_

This PR adds a `as_slice` method to all the string nodes which returns all the parts of the nodes as a slice. This will be useful in the next PR to split the string formatting to use this method to extract the _single node_ or _implicitly concanated nodes_.


---

_Comment by @dhruvmanila on 2023-12-13 03:01_

Current dependencies on/for this PR:
* `main`
  * **PR #9111** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9111?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #9058** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9058?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9111?utm_source=stack-comment).

---

_Label `internal` added by @dhruvmanila on 2023-12-13 03:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1055 on 2023-12-13 03:03_

Do we still need the `parts` method` or can we replae it with `as_slice` (same for `parts_mut` that we could replace with `as_slice_mut`?

---

_@MichaReiser approved on 2023-12-13 03:03_

---

_@dhruvmanila reviewed on 2023-12-13 03:05_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1055 on 2023-12-13 03:05_

We could which will require updating all the references to use `as_slice().iter()`. I think it should be a quick change. I'll do it before merging this PR, thanks for the suggestion.

---

_Comment by @github-actions[bot] on 2023-12-13 03:16_

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

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1055 on 2023-12-13 03:17_

We can also rename the methods to `iter` and return `std::slice::Iter` instead of `impl Iterator`. It would then make sense to possibly also implement `IntoIterator` (which is common for types implementing `as_slice()` and `iter`)

---

_@MichaReiser reviewed on 2023-12-13 03:17_

---

_Merged by @dhruvmanila on 2023-12-13 06:31_

---

_Closed by @dhruvmanila on 2023-12-13 06:31_

---

_Branch deleted on 2023-12-13 06:31_

---
