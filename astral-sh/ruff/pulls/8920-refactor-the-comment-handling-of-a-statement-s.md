```yaml
number: 8920
title: "Refactor the comment handling of a statement's last expression"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-last-statement-expression-comment-formatting
created_at: 2023-11-30T02:57:00Z
updated_at: 2023-12-12T03:09:10Z
url: https://github.com/astral-sh/ruff/pull/8920
synced_at: 2026-01-10T23:40:55Z
```

# Refactor the comment handling of a statement's last expression

---

_Pull request opened by @MichaReiser on 2023-11-30 02:57_

## Summary

This refactor simplifies implementing https://github.com/astral-sh/ruff/issues/6975. I also find it easier to understand where this layout is used because the explicit calls in the assignment formatting compared to inside (anyway complicated) `maybe_parenthesize` function.

## Test Plan

`cargo test`

The similarity index remains unchanged

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99963 |             10596 |               146 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               322 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                21 |


---

_Comment by @MichaReiser on 2023-11-30 02:57_

Current dependencies on/for this PR:
* main
  * **PR #8920** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8920?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #8943** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8943?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #9102** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9102?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8941** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8941?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8940** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8940?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8920?utm_source=stack-comment).

---

_Renamed from "Refactor the comment handling of a statement's last expression" to "WIP: Refactor the comment handling of a statement's last expression" by @MichaReiser on 2023-11-30 02:57_

---

_Label `internal` added by @MichaReiser on 2023-11-30 02:57_

---

_Comment by @github-actions[bot] on 2023-11-30 03:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "WIP: Refactor the comment handling of a statement's last expression" to "Refactor the comment handling of a statement's last expression" by @MichaReiser on 2023-12-01 04:42_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-01 04:42_

---

_Marked ready for review by @MichaReiser on 2023-12-01 04:43_

---

_@charliermarsh approved on 2023-12-01 04:59_

---

_Merged by @MichaReiser on 2023-12-04 05:12_

---

_Closed by @MichaReiser on 2023-12-04 05:12_

---

_Branch deleted on 2023-12-04 05:12_

---
