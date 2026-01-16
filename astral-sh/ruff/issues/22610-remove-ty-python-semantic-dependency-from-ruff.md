```yaml
number: 22610
title: "Remove `ty_python_semantic` dependency from `ruff_graph`"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2026-01-15T20:17:59Z
updated_at: 2026-01-16T17:31:32Z
url: https://github.com/astral-sh/ruff/issues/22610
synced_at: 2026-01-16T18:00:50Z
```

# Remove `ty_python_semantic` dependency from `ruff_graph`

---

_@MichaReiser_

The only reason `ruff_graph` depends on the very heavyweight `ty_python_semantic` crate is because of its dependency on `PythonEnvironment`. 

We could move `PythonEnvironment` to `ty_module_resolver` or its own crate (that does all virtual env stuff), so that `ruff_graph` only depends on `ty_module_resolver` and that new crate. This should help improve build times. It might also help with binary size.

---

_Label `internal` added by @MichaReiser on 2026-01-15 20:18_

---

_Comment by @AlexWaygood on 2026-01-16 17:22_

fixed in https://github.com/astral-sh/ruff/pull/22623

---

_Closed by @AlexWaygood on 2026-01-16 17:22_

---

_Comment by @MichaReiser on 2026-01-16 17:31_

Thank you

---
