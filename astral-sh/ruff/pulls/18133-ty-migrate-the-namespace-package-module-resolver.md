```yaml
number: 18133
title: "[ty] Migrate the namespace package module resolver tests to mdtests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/namespace-mdtest
created_at: 2025-05-16T14:03:56Z
updated_at: 2025-05-16T17:56:35Z
url: https://github.com/astral-sh/ruff/pull/18133
synced_at: 2026-01-12T15:56:13Z
```

# [ty] Migrate the namespace package module resolver tests to mdtests

---

_@MichaReiser_

_No description provided._

---

_Review requested from @carljm by @MichaReiser on 2025-05-16 14:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-16 14:03_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-16 14:03_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-16 14:03_

---

_Label `testing` added by @MichaReiser on 2025-05-16 14:04_

---

_Label `ty` added by @MichaReiser on 2025-05-16 14:04_

---

_Comment by @github-actions[bot] on 2025-05-16 14:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/namespace.md`:43 on 2025-05-16 17:31_

```suggestion
An adapted test case from the
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/namespace.md`:54 on 2025-05-16 17:33_

```suggestion
.venv/site-packages
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/namespace.md`:104 on 2025-05-16 17:34_

```suggestion
reveal_type(x)  # revealed: Unknown | Literal["module"]

import foo.bar  # error: [unresolved-import]
```

---

_@AlexWaygood approved on 2025-05-16 17:36_

---

_Merged by @MichaReiser on 2025-05-16 17:56_

---

_Closed by @MichaReiser on 2025-05-16 17:56_

---

_Branch deleted on 2025-05-16 17:56_

---
