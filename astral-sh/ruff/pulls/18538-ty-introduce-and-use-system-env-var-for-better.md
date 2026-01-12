```yaml
number: 18538
title: "[ty] Introduce and use `System::env_var` for better test isolation"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/system-env-var
created_at: 2025-06-07T17:30:49Z
updated_at: 2025-06-07T17:57:00Z
url: https://github.com/astral-sh/ruff/pull/18538
synced_at: 2026-01-12T15:56:21Z
```

# [ty] Introduce and use `System::env_var` for better test isolation

---

_@MichaReiser_

## Summary

Our venv discovery is too good. The corpus tests can now fail if a virtual environment is activated
because ty picks up the `VIRTUAL_ENV` environment variable.

This PR adds a new `env_var` method to `System`, which ensures tests are fully isolated from the host environment (at least when using a `TestSystem`)

## Test Plan

The corpus tests no longer fail when inside a venv


---

_Review requested from @carljm by @MichaReiser on 2025-06-07 17:30_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-07 17:30_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-07 17:30_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-07 17:30_

---

_Renamed from "[ty] Introduce `System::env_var` for better test isolation" to "[ty] Introduce and use `System::env_var` for better test isolation" by @MichaReiser on 2025-06-07 17:31_

---

_Label `testing` added by @MichaReiser on 2025-06-07 17:31_

---

_Label `ty` added by @MichaReiser on 2025-06-07 17:31_

---

_Comment by @github-actions[bot] on 2025-06-07 17:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-06-07 17:44_

---

_Merged by @MichaReiser on 2025-06-07 17:56_

---

_Closed by @MichaReiser on 2025-06-07 17:56_

---

_Branch deleted on 2025-06-07 17:56_

---
