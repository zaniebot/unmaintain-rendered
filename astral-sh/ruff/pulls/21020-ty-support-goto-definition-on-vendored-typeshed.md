```yaml
number: 21020
title: "[ty] Support goto-definition on vendored typeshed stubs"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/goto-typeshed2
created_at: 2025-10-21T16:02:47Z
updated_at: 2025-10-21T17:38:43Z
url: https://github.com/astral-sh/ruff/pull/21020
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Support goto-definition on vendored typeshed stubs

---

_@Gankra_

This is an alternative to #21012 that more narrowly handles this logic in the stub-mapping machinery rather than pervasively allowing us to identify cached files as typeshed stubs. Much of the logic is the same (pulling the logic out of ty_server so it can be reused).

I don't have a good sense for if one approach is "better" or "worse" in terms of like, semantics and Weird Bugs that this can cause. This one is just "less spooky in its broad consequences" and "less muddying of separation of concerns" and puts the extra logic on a much colder path. I won't be surprised if one day the previous implementation needs to be revisited for its more sweeping effects but for now this is good.

Fixes https://github.com/astral-sh/ty/issues/1054

---

_Review requested from @carljm by @Gankra on 2025-10-21 16:02_

---

_Label `server` added by @Gankra on 2025-10-21 16:02_

---

_Review requested from @AlexWaygood by @Gankra on 2025-10-21 16:02_

---

_Review requested from @sharkdp by @Gankra on 2025-10-21 16:02_

---

_Review requested from @dcreager by @Gankra on 2025-10-21 16:02_

---

_Review requested from @MichaReiser by @Gankra on 2025-10-21 16:02_

---

_Label `ty` added by @Gankra on 2025-10-21 16:02_

---

_Comment by @github-actions[bot] on 2025-10-21 16:05_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 16:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@Gankra reviewed on 2025-10-21 16:06_

---

_Review comment by @Gankra on `crates/ty_ide/src/lib.rs`:298 on 2025-10-21 16:06_

This is morally a const fn but I didn't want to deal with making that work and it's not a huge deal.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/lib.rs`:298 on 2025-10-21 16:20_

haha, I wanted something similar recently too and did draw the very same conclusion: I don't want to deal with this right now :)

---

_Comment by @github-actions[bot] on 2025-10-21 16:20_

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

_Review comment by @MichaReiser on `crates/ty_ide/src/lib.rs`:297 on 2025-10-21 16:23_

```suggestion
    SystemPath::from(concat!("vendored/typeshed", ty_vendored::SOURCE_COMMIT))
```

---

_@MichaReiser approved on 2025-10-21 16:25_

I like that. I agree that it's hard to predict if there are cases where it will break, but we'll see. Thanks for giving this a try!

---

_@Gankra reviewed on 2025-10-21 17:38_

---

_Review comment by @Gankra on `crates/ty_ide/src/lib.rs`:297 on 2025-10-21 17:38_

Unfortunately `concat` concatenates only literals so this is a compiler error :(

---

_Merged by @Gankra on 2025-10-21 17:38_

---

_Closed by @Gankra on 2025-10-21 17:38_

---

_Branch deleted on 2025-10-21 17:38_

---
