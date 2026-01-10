```yaml
number: 9128
title: "Add new `with` and `match` sequence test cases"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: new-with-match-sequence-tests
created_at: 2023-12-14T10:48:38Z
updated_at: 2023-12-15T02:47:51Z
url: https://github.com/astral-sh/ruff/pull/9128
synced_at: 2026-01-10T23:31:11Z
```

# Add new `with` and `match` sequence test cases

---

_Pull request opened by @MichaReiser on 2023-12-14 10:48_

## Summary

Add new test cases for `with_item` and `match` sequence that demonstrate how long headers break. 

Removes one use of `optional_parentheses` in a position where it is know that the parentheses always need to be added.

## Test Plan

cargo test


---

_Comment by @MichaReiser on 2023-12-14 10:48_

Current dependencies on/for this PR:
* `main`
  * **PR #9127** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9127?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #9128** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9128?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9128?utm_source=stack-comment).

---

_@charliermarsh approved on 2023-12-14 18:43_

---

_Label `internal` added by @MichaReiser on 2023-12-15 01:59_

---

_Merged by @MichaReiser on 2023-12-15 02:45_

---

_Closed by @MichaReiser on 2023-12-15 02:45_

---

_Comment by @MichaReiser on 2023-12-15 02:45_

### Merge activity

* **Dec 14, 9:45 PM**: @@MichaReiser merged this pull request with [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9128).


---

_Branch deleted on 2023-12-15 02:45_

---

_Comment by @github-actions[bot] on 2023-12-15 02:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
