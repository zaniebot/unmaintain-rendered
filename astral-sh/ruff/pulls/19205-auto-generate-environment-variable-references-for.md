```yaml
number: 19205
title: Auto-generate environment variable references for ty
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: charlie/env-var
created_at: 2025-07-08T13:32:09Z
updated_at: 2025-07-08T14:50:39Z
url: https://github.com/astral-sh/ruff/pull/19205
synced_at: 2026-01-12T15:56:34Z
```

# Auto-generate environment variable references for ty

---

_@charliermarsh_

## Summary

This PR mirrors the environment variable implementation we have in uv: https://github.com/astral-sh/uv/blob/efc361223c134840b7650fed1ef9ebd9577b5454/crates/uv-static/src/env_vars.rs#L6-L7.

See: https://github.com/astral-sh/ty/issues/773.


---

_Review requested from @carljm by @charliermarsh on 2025-07-08 13:32_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-07-08 13:32_

---

_Review requested from @sharkdp by @charliermarsh on 2025-07-08 13:32_

---

_Review requested from @dcreager by @charliermarsh on 2025-07-08 13:32_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-07-08 13:32_

---

_Comment by @github-actions[bot] on 2025-07-08 13:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-08 13:47_

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

_@AlexWaygood approved on 2025-07-08 13:49_

Nice!

---

_Label `internal` added by @AlexWaygood on 2025-07-08 13:51_

---

_Label `ty` added by @AlexWaygood on 2025-07-08 13:51_

---

_Review comment by @MichaReiser on `crates/ty_macros/Cargo.toml`:10 on 2025-07-08 14:06_

We currently have all ty macros in `ruff_macros`.

---

_@MichaReiser reviewed on 2025-07-08 14:06_

---

_@MichaReiser reviewed on 2025-07-08 14:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/tests/mdtest.rs`:23 on 2025-07-08 14:07_

This environment variable isn't exposed to users. It's only used for testing. I see, you use `#[attribute_hidden]` for this. I guess that works. 

---

_@AlexWaygood reviewed on 2025-07-08 14:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/tests/mdtest.rs`:23 on 2025-07-08 14:09_

the variant has the `#[attr_hidden]` attribute on it, so it won't appear in the generated docs

---

_Review comment by @MichaReiser on `crates/ty/docs/environment.md`:1 on 2025-07-08 14:09_

We'll need to make sure to update the ty release script to copy over the new file.

---

_@MichaReiser approved on 2025-07-08 14:09_

Thank you

---

_@AlexWaygood reviewed on 2025-07-08 14:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/tests/mdtest.rs`:23 on 2025-07-08 14:10_

oops, I sent my comment before I saw your edit :-)

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-08 14:18_

---

_Merged by @charliermarsh on 2025-07-08 14:48_

---

_Closed by @charliermarsh on 2025-07-08 14:48_

---

_Branch deleted on 2025-07-08 14:48_

---
