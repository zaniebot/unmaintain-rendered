```yaml
number: 193
title: Support system python installations
type: issue
state: closed
author: MichaReiser
labels:
  - imports
assignees: []
created_at: 2025-02-24T18:15:56Z
updated_at: 2025-05-11T15:55:03Z
url: https://github.com/astral-sh/ty/issues/193
synced_at: 2026-01-12T15:54:22Z
```

# Support system python installations

---

_@MichaReiser_

### Description

https://github.com/astral-sh/ruff/pull/16347 renamed the `venv-path` option to `python` to align the option name with uv. However, Red Knot still only supports virtual environment paths but not paths to system pythons (because it tests for the presence of a `pyvenv.cfg`)

https://github.com/astral-sh/ruff/blob/33d78d3f15bc7368bcd94daf8a2a6117565c6837/crates/red_knot_python_semantic/src/module_resolver/resolver.rs#L226-L233

We should extend the paths supported for the `python` options to support system python installations and, optionally, to take paths to python executables (So that you can `which python`)

---

_Renamed from "[red-knot] Support system python installations" to "Support system python installations" by @MichaReiser on 2025-05-07 15:26_

---

_Assigned to @zanieb by @AlexWaygood on 2025-05-11 07:26_

---

_Label `imports` added by @AlexWaygood on 2025-05-11 07:51_

---

_Comment by @zanieb on 2025-05-11 15:55_

Closed in https://github.com/astral-sh/ruff/pull/17991

Let's track 

> to take paths to python executables (So that you can which python)

separately

---

_Closed by @zanieb on 2025-05-11 15:55_

---
