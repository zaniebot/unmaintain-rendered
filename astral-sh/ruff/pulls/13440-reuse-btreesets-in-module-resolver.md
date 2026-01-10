```yaml
number: 13440
title: "Reuse `BTreeSets` in module resolver"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/reuse
created_at: 2024-09-21T18:23:53Z
updated_at: 2024-09-21T20:23:32Z
url: https://github.com/astral-sh/ruff/pull/13440
synced_at: 2026-01-10T21:08:14Z
```

# Reuse `BTreeSets` in module resolver

---

_Pull request opened by @charliermarsh on 2024-09-21 18:23_

## Summary

For dependencies, there's no reason to re-allocate here, since we know the paths are unique.


---

_Label `performance` added by @charliermarsh on 2024-09-21 18:30_

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/lib.rs`:94 on 2024-09-21 18:38_

Would you mind naming the fields? The code is now kind of hard to understand where the types are generic data structures and the fields are not named 

---

_@MichaReiser reviewed on 2024-09-21 18:39_

---

_Comment by @github-actions[bot] on 2024-09-21 18:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-21 18:57_

---

_Review comment by @MichaReiser on `crates/ruff_graph/src/lib.rs`:94 on 2024-09-21 18:57_

You could also use File as values if you happen to have interned anyway. 

A File is just a u32, so super fast to hash

---

_@charliermarsh reviewed on 2024-09-21 19:00_

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/lib.rs`:94 on 2024-09-21 19:00_

What kinds of names would help here? Do you mean using named type aliases or named fields?

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/lib.rs`:94 on 2024-09-21 19:02_

Oh wow, smart. I do need it to be sorted / ordered in the output though 🤔 

---

_@charliermarsh reviewed on 2024-09-21 19:02_

---

_@charliermarsh reviewed on 2024-09-21 19:14_

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/lib.rs`:94 on 2024-09-21 19:14_

I could try doing everything on u32 and then converting to sorted system paths at the very end

---

_@charliermarsh reviewed on 2024-09-21 20:10_

---

_Review comment by @charliermarsh on `crates/ruff_graph/src/lib.rs`:94 on 2024-09-21 20:10_

(I just retained the use of the struct here.)

---

_Merged by @charliermarsh on 2024-09-21 20:14_

---

_Closed by @charliermarsh on 2024-09-21 20:14_

---

_Branch deleted on 2024-09-21 20:14_

---
