```yaml
number: 20413
title: "[ty] Allow annotation expressions to be `ast::Attribute` nodes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/classvar-attr
created_at: 2025-09-15T10:52:08Z
updated_at: 2025-09-15T11:06:50Z
url: https://github.com/astral-sh/ruff/pull/20413
synced_at: 2026-01-12T15:57:01Z
```

# [ty] Allow annotation expressions to be `ast::Attribute` nodes

---

_@AlexWaygood_

Fixes https://github.com/astral-sh/ty/issues/1187

---

_Review requested from @carljm by @AlexWaygood on 2025-09-15 10:52_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-15 10:52_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-15 10:52_

---

_Label `ty` added by @AlexWaygood on 2025-09-15 10:52_

---

_Comment by @github-actions[bot] on 2025-09-15 10:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-15 10:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_iter.py:34:7: error[invalid-type-form] Type qualifier `typing.Final` is not allowed in type expressions (only in annotation expressions)
- Found 14 diagnostics
+ Found 13 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_tests/test_ssl.py:217:25: warning[possibly-unbound-attribute] Attribute `SSL_OP_NO_TLSv1_3` on type `Unknown | None` is possibly unbound
- Found 675 diagnostics
+ Found 676 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-09-15 11:03_

Thank you!

---

_Merged by @AlexWaygood on 2025-09-15 11:06_

---

_Closed by @AlexWaygood on 2025-09-15 11:06_

---

_Branch deleted on 2025-09-15 11:06_

---
