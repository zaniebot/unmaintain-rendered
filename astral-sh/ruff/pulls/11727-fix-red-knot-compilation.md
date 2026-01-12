```yaml
number: 11727
title: "Fix `red-knot` compilation"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-06-04T03:00:12Z
updated_at: 2024-06-04T03:39:23Z
url: https://github.com/astral-sh/ruff/pull/11727
synced_at: 2026-01-12T15:55:38Z
```

# Fix `red-knot` compilation

---

_@charliermarsh_

## Summary

Perhaps a result of a bad rebase, but `cargo clippy --fix --workspace --all-targets -- -D warnings` does not pass on main as-is.


---

_Review requested from @carljm by @charliermarsh on 2024-06-04 03:00_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-06-04 03:00_

---

_@charliermarsh reviewed on 2024-06-04 03:00_

---

_Review comment by @charliermarsh on `crates/red_knot/src/types/infer.rs`:221 on 2024-06-04 03:00_

Some of these symbols were missing.

---

_@charliermarsh reviewed on 2024-06-04 03:00_

---

_Review comment by @charliermarsh on `crates/red_knot/src/types/infer.rs`:392 on 2024-06-04 03:00_

This is defined twice. I assume the second version is a superset of it.

---

_Merged by @charliermarsh on 2024-06-04 03:03_

---

_Closed by @charliermarsh on 2024-06-04 03:03_

---

_Branch deleted on 2024-06-04 03:03_

---

_Comment by @github-actions[bot] on 2024-06-04 03:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-06-04 03:36_

Sorry! I thought CI was green on the PRs I merged, but there must have been one that didn't merge well into main. Thanks for fixing. 

---

_Comment by @charliermarsh on 2024-06-04 03:39_

No prob don't sweat it.

---
