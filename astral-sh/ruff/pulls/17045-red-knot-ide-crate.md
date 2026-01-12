```yaml
number: 17045
title: "[red-knot] IDE crate"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/knot-ide
created_at: 2025-03-28T19:17:32Z
updated_at: 2025-04-01T07:36:03Z
url: https://github.com/astral-sh/ruff/pull/17045
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] IDE crate

---

_@MichaReiser_

## Summary

This PR adds a new but so far empty and unused `red_knot_ide` crate. 

This new crate's purpose is to implement IDE-specific functionality, such as go to definition, hover, completion, etc., which are used by both the LSP and the playground.

The crate itself doesn't depend on `lsptypes`. The idea is that the facade crates (e.g., `red_knot_server`) convert external to internal types. 
Not only allows this to share the logic between server and playground, it also ensures that the core functionality is easier to test because it can be tested without needing a full LSP.



## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2025-03-28 19:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-28 19:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-28 19:17_

---

_Review requested from @dcreager by @MichaReiser on 2025-03-28 19:17_

---

_Label `red-knot` added by @MichaReiser on 2025-03-28 19:17_

---

_Comment by @github-actions[bot] on 2025-03-28 19:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-03-28 19:26_

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

_@charliermarsh approved on 2025-03-28 19:27_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-28 19:28_

---

_Comment by @MichaReiser on 2025-03-28 20:30_

@charliermarsh I don't know if you have enough authority to approve red knot PRs 😂 

---

_Comment by @charliermarsh on 2025-03-28 20:53_

Watch me!

---

_@carljm approved on 2025-03-30 19:04_

I skipped reviewing this last week because @charliermarsh already approved it! But if you are serious about wanting another review, it looks fine to me 😆 

---

_Merged by @MichaReiser on 2025-04-01 07:36_

---

_Closed by @MichaReiser on 2025-04-01 07:36_

---

_Branch deleted on 2025-04-01 07:36_

---
