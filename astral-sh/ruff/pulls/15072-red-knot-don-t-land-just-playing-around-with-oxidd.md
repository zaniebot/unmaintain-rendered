```yaml
number: 15072
title: "[red-knot] don't land, just playing around with oxidd"
type: pull_request
state: closed
author: carljm
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: cjm/oxidd
created_at: 2024-12-19T19:46:06Z
updated_at: 2025-02-14T19:01:57Z
url: https://github.com/astral-sh/ruff/pull/15072
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] don't land, just playing around with oxidd

---

_Pull request opened by @carljm on 2024-12-19 19:46_

Run this using `cargo run --bin red_knot`; this PR replaces the entire red-knot CLI with outputting a graph visualization file instead, because it was the laziest possible way I could do this :)

To create a png file from the `tdd.dot` file this outputs, I used `dot -Tpng tdd.dot -o tdd.png`, using `dot` CLI from the GraphViz package.


---

_Comment by @github-actions[bot] on 2024-12-19 19:55_

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

_Label `do-not-merge` added by @AlexWaygood on 2024-12-19 20:59_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-19 20:59_

---

_Closed by @carljm on 2025-02-14 19:01_

---
