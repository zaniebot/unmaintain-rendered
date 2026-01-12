```yaml
number: 12715
title: "[red-knot] Keep subcommands optional for the binary"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/red-knot-server-opt
created_at: 2024-08-06T14:16:39Z
updated_at: 2024-08-06T14:54:51Z
url: https://github.com/astral-sh/ruff/pull/12715
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Keep subcommands optional for the binary

---

_@dhruvmanila_

## Summary

This PR updates the `red_knot` CLI to make the subcommand optional.

## Test Plan

Run the following commands:
* `cargo run --bin red_knot -- --current-directory=~/playground/ruff/type_inference` (no subcommand requirement)
* `cargo run --bin red_knot -- server` (should start the server)


---

_Label `red-knot` added by @dhruvmanila on 2024-08-06 14:16_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-06 14:16_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-06 14:16_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-06 14:16_

---

_Comment by @github-actions[bot] on 2024-08-06 14:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-06 14:46_

Thanks

---

_Merged by @dhruvmanila on 2024-08-06 14:54_

---

_Closed by @dhruvmanila on 2024-08-06 14:54_

---

_Branch deleted on 2024-08-06 14:54_

---
