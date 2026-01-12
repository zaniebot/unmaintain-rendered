```yaml
number: 18226
title: Update salsa
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
assignees: []
draft: true
base: main
head: ibraheem/interned-gc
created_at: 2025-05-20T17:38:31Z
updated_at: 2025-06-11T21:38:51Z
url: https://github.com/astral-sh/ruff/pull/18226
synced_at: 2026-01-12T15:56:15Z
```

# Update salsa

---

_@ibraheemdev_

## Summary

---

_Label `ty` added by @ibraheemdev on 2025-05-20 17:38_

---

_Comment by @github-actions[bot] on 2025-05-20 17:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-20 18:34_

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

_@ibraheemdev reviewed on 2025-05-20 19:14_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-20 19:14_

This change is a little odd, maybe something to do with the ID generation affecting some sort order?

---

_@sharkdp reviewed on 2025-05-20 19:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-20 19:39_

Something was non-deterministic here before, so it's not related to your changes.

---

_@AlexWaygood reviewed on 2025-05-21 22:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-21 22:08_

I switched to using an `IndexMap` in https://github.com/astral-sh/ruff/commit/cb04343b3b5e7a8a0841c73537733fa5aac482a2 to try to improve the determinism of the order of diagnostics -- can you try rebasing and see if the order still changes after the rebase?

---

_@ibraheemdev reviewed on 2025-05-27 22:49_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-27 22:49_

Looks like that fixed it, thanks!

---

_@AlexWaygood reviewed on 2025-05-27 23:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/mro.md_-_Method_Resolution_Or…_-_`__bases__`_lists_wi…_(ea7ebc83ec359b54).snap`:144 on 2025-05-27 23:23_

Glad to hear it 😃

---

_Closed by @ibraheemdev on 2025-06-11 21:38_

---

_Branch deleted on 2025-06-11 21:38_

---
