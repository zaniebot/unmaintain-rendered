```yaml
number: 16102
title: Fix release build warning about unused todo type message
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/knot-release-compile-warning
created_at: 2025-02-11T18:34:38Z
updated_at: 2025-02-11T18:38:42Z
url: https://github.com/astral-sh/ruff/pull/16102
synced_at: 2026-01-10T19:57:22Z
```

# Fix release build warning about unused todo type message

---

_Pull request opened by @MichaReiser on 2025-02-11 18:34_

_No description provided._

---

_Review requested from @carljm by @MichaReiser on 2025-02-11 18:34_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-11 18:34_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-11 18:34_

---

_Label `internal` added by @MichaReiser on 2025-02-11 18:34_

---

_Label `red-knot` added by @MichaReiser on 2025-02-11 18:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:43 on 2025-02-11 18:35_

```suggestion
    #[allow(unused_variables)]  // only unused in release builds
```

---

_@AlexWaygood approved on 2025-02-11 18:35_

---

_Merged by @MichaReiser on 2025-02-11 18:38_

---

_Closed by @MichaReiser on 2025-02-11 18:38_

---

_Branch deleted on 2025-02-11 18:38_

---
