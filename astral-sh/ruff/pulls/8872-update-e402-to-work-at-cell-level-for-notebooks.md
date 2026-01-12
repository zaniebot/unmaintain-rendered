```yaml
number: 8872
title: "Update `E402` to work at cell level for notebooks"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/e402-notebook
created_at: 2023-11-28T18:02:15Z
updated_at: 2023-11-29T00:42:00Z
url: https://github.com/astral-sh/ruff/pull/8872
synced_at: 2026-01-12T15:55:27Z
```

# Update `E402` to work at cell level for notebooks

---

_@dhruvmanila_

## Summary

This PR updates the `E402` rule to work at cell level for Jupyter notebooks. This is enabled only in preview to gather feedback.

The implementation basically resets the import boundary flag on the semantic model when we encounter the first statement in a cell.

Another potential solution is to introduce `E403` rule that is specifically for notebooks that works at cell level while `E402` will be disabled for notebooks.

## Test Plan

Add a notebook with imports in multiple cells and verify that the rule works as expected.

resolves: #8669 


---

_Comment by @dhruvmanila on 2023-11-28 18:02_

Current dependencies on/for this PR:
* main
  * **PR #8872** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8872?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8872?utm_source=stack-comment).

---

_Label `rule` added by @dhruvmanila on 2023-11-28 18:02_

---

_Comment by @github-actions[bot] on 2023-11-28 18:24_

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

_@charliermarsh reviewed on 2023-11-28 21:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:286 on 2023-11-28 21:42_

Is `-=` equivalent here? It looks like we use that elsewhere in the file.

---

_@charliermarsh reviewed on 2023-11-28 21:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/mod.rs`:276 on 2023-11-28 21:45_

I think it's probably fine not to gate this on preview.

---

_@charliermarsh approved on 2023-11-28 21:45_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:286 on 2023-11-29 00:22_

Oh, I didn't notice that. Although they're equivalent, I'll change it to `-=` for consistency.

---

_@dhruvmanila reviewed on 2023-11-29 00:22_

---

_Merged by @dhruvmanila on 2023-11-29 00:32_

---

_Closed by @dhruvmanila on 2023-11-29 00:32_

---

_Branch deleted on 2023-11-29 00:32_

---
