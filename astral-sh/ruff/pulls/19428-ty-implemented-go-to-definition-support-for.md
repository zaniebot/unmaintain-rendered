```yaml
number: 19428
title: "[ty] Implemented \"go to definition\" support for import statements"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: goto_declaration_modules
created_at: 2025-07-19T01:27:39Z
updated_at: 2025-07-19T18:22:14Z
url: https://github.com/astral-sh/ruff/pull/19428
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Implemented "go to definition" support for import statements

---

_Pull request opened by @UnboundVariable on 2025-07-19 01:27_

This PR extends the "go to declaration" and "go to definition" functionality to support import statements — both standard imports and "from" import forms.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-19 01:27_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-19 01:27_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-19 01:27_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-19 01:27_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-19 01:27_

---

_Comment by @github-actions[bot] on 2025-07-19 01:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-07-19 13:25_

---

_Label `ty` added by @MichaReiser on 2025-07-19 13:25_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:441 on 2025-07-19 13:37_

You can use `covering_node.ancestors` instead of doing the entire tree traversal again. See https://github.com/astral-sh/ruff/pull/19429

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:449 on 2025-07-19 13:39_

```suggestion
                            if asname.range.contains(offset) {
```

or `.contains_inclusive` if you want to include the end as well (which is one passed the alias)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:225 on 2025-07-19 13:40_

Same as in your other PR. Let's extract the `ImportFrom` statement in `find_goto_target`, See https://github.com/astral-sh/ruff/pull/19429

---

_@MichaReiser approved on 2025-07-19 13:43_

---

_@UnboundVariable reviewed on 2025-07-19 16:36_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/goto.rs`:449 on 2025-07-19 16:36_

Nice. I didn't know about `contains_inclusive`. I can use this in multiple places.

---

_Merged by @UnboundVariable on 2025-07-19 18:22_

---

_Closed by @UnboundVariable on 2025-07-19 18:22_

---

_Branch deleted on 2025-07-19 18:22_

---
