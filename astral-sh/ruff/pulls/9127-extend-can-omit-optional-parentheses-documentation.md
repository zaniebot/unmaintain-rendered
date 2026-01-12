```yaml
number: 9127
title: "Extend `can_omit_optional_parentheses` documentation"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: 12-14-Extend_can_omit_optional_parentheses_documentation
created_at: 2023-12-14T10:28:39Z
updated_at: 2023-12-15T02:20:20Z
url: https://github.com/astral-sh/ruff/pull/9127
synced_at: 2026-01-12T15:55:27Z
```

# Extend `can_omit_optional_parentheses` documentation

---

_@MichaReiser_


## Summary

Add some more documentation to `can_omit_optional_parentheses` because it is realy hard to understand.
Restrict the `Attribute` and `None` `OperatorPrecedence` branches to ensure they only get applyied to the intended nodes.

## Test Plan

Ecosystem check reports no differences. The compatibility index remains unchanged. 


---

_Comment by @MichaReiser on 2023-12-14 10:28_

Current dependencies on/for this PR:
* `main`
  * **PR #9127** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9127?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #9128** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9128?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9127?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-12-14 11:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:540 on 2023-12-14 18:42_

Just confirming it's intentional that the order of the branches switched here.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:588 on 2023-12-14 18:42_

Nit: newline :)

---

_@charliermarsh approved on 2023-12-14 18:42_

---

_Label `internal` added by @MichaReiser on 2023-12-15 01:59_

---

_@MichaReiser reviewed on 2023-12-15 02:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:540 on 2023-12-15 02:02_

Not in that it has (or should have) any behavioral change. I just tried to identify which rules apply to all (e.g. `max_precedence_count > 1` cases and which cases are "special"

---

_Merged by @MichaReiser on 2023-12-15 02:18_

---

_Closed by @MichaReiser on 2023-12-15 02:18_

---

_Comment by @MichaReiser on 2023-12-15 02:18_

### Merge activity

* **Dec 14, 9:18 PM**: @@MichaReiser merged this pull request with [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9127).


---

_Branch deleted on 2023-12-15 02:18_

---
