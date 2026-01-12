```yaml
number: 8815
title: "Avoid `B015`,`B018` for last expression in a cell"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/notebook-b015-b018
created_at: 2023-11-21T22:19:15Z
updated_at: 2023-11-22T15:39:11Z
url: https://github.com/astral-sh/ruff/pull/8815
synced_at: 2026-01-12T15:55:27Z
```

# Avoid `B015`,`B018` for last expression in a cell

---

_@dhruvmanila_

## Summary

This PR updates `B015` and `B018` to ignore last top-level expressions in each
cell of a Jupyter Notebook.

Part of #8669

## Test Plan

Add test cases for both rules and update the snapshots.


---

_Comment by @dhruvmanila on 2023-11-21 22:19_

Current dependencies on/for this PR:
* main
  * **PR #8813** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8813?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8814** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8814?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8815** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8815?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8815?utm_source=stack-comment).

---

_Label `rule` added by @dhruvmanila on 2023-11-21 22:20_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/helpers.rs`:22 on 2023-11-21 23:45_

Maybe a bit easier to understand as `.all(|token| token.kind() == SimpleTokenKind::Semi || token.kind().is_trivia())`? Right now, it's a double negative with `!any` and the internal `!=`.

---

_@charliermarsh reviewed on 2023-11-21 23:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/helpers.rs`:10 on 2023-11-21 23:45_

Can probably be `pub(super)`?

---

_@charliermarsh reviewed on 2023-11-21 23:45_

---

_@charliermarsh approved on 2023-11-21 23:45_

Great!

---

_@dhruvmanila reviewed on 2023-11-22 15:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bugbear/helpers.rs`:22 on 2023-11-22 15:24_

Thanks! My initial implementation was using `skip_while` which clippy didn't like. This is much better.

---

_Merged by @dhruvmanila on 2023-11-22 15:33_

---

_Closed by @dhruvmanila on 2023-11-22 15:33_

---

_Branch deleted on 2023-11-22 15:33_

---

_Comment by @github-actions[bot] on 2023-11-22 15:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
