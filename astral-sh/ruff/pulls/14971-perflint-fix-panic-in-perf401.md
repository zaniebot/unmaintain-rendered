```yaml
number: 14971
title: "[`perflint`] Fix panic in `perf401`"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: perf401_panic
created_at: 2024-12-14T15:57:42Z
updated_at: 2024-12-15T15:22:04Z
url: https://github.com/astral-sh/ruff/pull/14971
synced_at: 2026-01-10T20:42:27Z
```

# [`perflint`] Fix panic in `perf401`

---

_Pull request opened by @w0nder1ng on 2024-12-14 15:57_

Fixes #14969.

The issue was that this line:

```rust
let from_assign_to_loop = TextRange::new(binding_stmt.end(), for_stmt.start());
```

was not safe if the binding was after the target. The only way (at least that I can think of) this can happen is if they are in different scopes, so it now checks for that before checking if there are usages between the two. 

---

_Comment by @github-actions[bot] on 2024-12-14 16:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-12-14 18:20_

Thanks! Could you add a snippet to one of the fixtures that triggers this panic on `main`?

---

_@MichaReiser approved on 2024-12-15 15:21_

Thanks

---

_Label `bug` added by @MichaReiser on 2024-12-15 15:22_

---

_Label `fixes` added by @MichaReiser on 2024-12-15 15:22_

---

_Label `preview` added by @MichaReiser on 2024-12-15 15:22_

---

_Merged by @MichaReiser on 2024-12-15 15:22_

---

_Closed by @MichaReiser on 2024-12-15 15:22_

---
