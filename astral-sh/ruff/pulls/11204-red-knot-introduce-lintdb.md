```yaml
number: 11204
title: "[red knot] Introduce `LintDb`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-lint-jar
created_at: 2024-04-29T13:49:52Z
updated_at: 2024-04-30T14:01:47Z
url: https://github.com/astral-sh/ruff/pull/11204
synced_at: 2026-01-12T15:55:37Z
```

# [red knot] Introduce `LintDb`

---

_@MichaReiser_

## Summary

This PR introduces a new `LintDb` and moves the `lint_syntax` and `lint_semantic` methods to the new trait. 

The reason for splitting out a new `LintDb` is that I see the following crates long term (at least)

* `red_knot_query`: Infrastructure for a query based compiler
* `red_knot_source`: Exposes the base queries to read a file and parse it.
* `red_knot_formatter`: Exposes format queries (I don't know how they will look and what we want to cache). Uses `red_knot_source` so that we can re-use parse trees between lint and format
* `red_knot_semantic` or `red_knot_analysis`: Contains the `SemanticDb` exposing the semantic model and type inference (I'm not yet sure where I would put type checking)
* `red_knot_lint`: Implements the lint queries. Is based on both `red_knot_source` and `red_knot_semantic`. Idealyly, we would have a way to split lint rules into sub-crates, but I'm not yet sure how that would work. 


## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-04-29 13:50_

---

_Comment by @github-actions[bot] on 2024-04-29 14:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @carljm by @MichaReiser on 2024-04-30 07:24_

---

_@carljm approved on 2024-04-30 13:53_

The proposed organization makes sense to me! I think we should be able to separate type checking from type inference, but not 100% sure how that will look until we get further on implementation.

---

_Merged by @MichaReiser on 2024-04-30 14:01_

---

_Closed by @MichaReiser on 2024-04-30 14:01_

---

_Branch deleted on 2024-04-30 14:01_

---
