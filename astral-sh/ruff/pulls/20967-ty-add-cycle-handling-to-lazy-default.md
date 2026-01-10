```yaml
number: 20967
title: "[ty] Add cycle handling to `lazy_default`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: micha/add-cycle-handling-to-lazy-defualt
created_at: 2025-10-19T08:52:18Z
updated_at: 2025-10-23T08:05:10Z
url: https://github.com/astral-sh/ruff/pull/20967
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Add cycle handling to `lazy_default`

---

_Pull request opened by @MichaReiser on 2025-10-19 08:52_

## Summary

Add cycle handling to `lazy_default`.

## Test Plan

Added regression test


---

_Label `bug` added by @MichaReiser on 2025-10-19 08:52_

---

_Label `ty` added by @MichaReiser on 2025-10-19 08:52_

---

_Label `bug` added by @MichaReiser on 2025-10-19 08:52_

---

_Label `ty` added by @MichaReiser on 2025-10-19 08:52_

---

_Comment by @github-actions[bot] on 2025-10-19 08:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-19 08:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-19 09:10_

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

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/primer/bad.txt`:1 on 2025-10-19 10:14_

The second of the two panics mentioned here is presumably gone after Doug's recent work in https://github.com/astral-sh/ruff/pull/20677?

---

_@AlexWaygood reviewed on 2025-10-19 10:14_

---

_@MichaReiser reviewed on 2025-10-23 06:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:316 on 2025-10-23 06:47_

This is a bit too advanced Python typing for me 😅 

I think it would be good to have some `reveal_type` or other assertion in this test but I need some help with what that assertion could be.

---

_Marked ready for review by @MichaReiser on 2025-10-23 06:49_

---

_Review requested from @carljm by @MichaReiser on 2025-10-23 06:49_

---

_Review requested from @sharkdp by @MichaReiser on 2025-10-23 06:49_

---

_Review requested from @dcreager by @MichaReiser on 2025-10-23 06:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:322 on 2025-10-23 07:21_

Minor:
```suggestion
from typing_extensions import Protocol, TypeVar
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:320 on 2025-10-23 07:22_

This shouldn't be necessary if `"C"` is a stringified annotation.
```suggestion
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:327 on 2025-10-23 07:24_

I think two assertions like these are probably sufficient here:
```suggestion
    pass

reveal_type(C[int]())  # revealed: C[int]
reveal_type(C())  # revealed: C[Divergent]
```

---

_@sharkdp approved on 2025-10-23 07:24_

Thank you!

---

_Merged by @MichaReiser on 2025-10-23 08:05_

---

_Closed by @MichaReiser on 2025-10-23 08:05_

---

_Branch deleted on 2025-10-23 08:05_

---
