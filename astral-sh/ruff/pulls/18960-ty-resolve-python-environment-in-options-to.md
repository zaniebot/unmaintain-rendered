```yaml
number: 18960
title: "[ty] Resolve python environment in `Options::to_program_settings`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/remove-python-environment-path
created_at: 2025-06-26T14:41:14Z
updated_at: 2025-06-26T16:02:27Z
url: https://github.com/astral-sh/ruff/pull/18960
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Resolve python environment in `Options::to_program_settings`

---

_@MichaReiser_

## Summary

This PR moves the discovery of the Python environment from `SearchPaths::from_settings` to `Options::to_program_settings`. This allows us to remove the `PythonEnvironmentPath` enum which we used to parametrize `SearchPaths::from_settings` whether it should discover the site packages paths because we didn't want that behavior in tests or when the user explicitly specified an environment. 

I didn't move `site_packages.rs` because we depend on it from `ty_test` and moving it to `ty_project` would require introducing a new dependency.

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @MichaReiser on 2025-06-26 14:41_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-26 14:41_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-26 14:41_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-26 14:41_

---

_Label `ty` added by @MichaReiser on 2025-06-26 14:41_

---

_Converted to draft by @MichaReiser on 2025-06-26 14:41_

---

_Comment by @github-actions[bot] on 2025-06-26 14:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `internal` added by @MichaReiser on 2025-06-26 14:50_

---

_@MichaReiser reviewed on 2025-06-26 14:51_

---

_Review comment by @MichaReiser on `crates/ruff/tests/analyze_graph.rs`:570 on 2025-06-26 14:51_

I could try to add those again but I actually don't find them very useful (the message on line 571 contains everything the user needs to know)

---

_Marked ready for review by @MichaReiser on 2025-06-26 14:56_

---

_Review request for @dcreager removed by @MichaReiser on 2025-06-26 14:56_

---

_Review request for @carljm removed by @MichaReiser on 2025-06-26 14:56_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-06-26 14:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:213 on 2025-06-26 15:00_

```suggestion
    /// (will only be available for virtual environments that specify
```

---

_Comment by @github-actions[bot] on 2025-06-26 15:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:215 on 2025-06-26 15:09_

I wonder if this should be called `python_version_from_metadata` since we also have `SitePackagesPaths::try_resolve_installation_python_version`, which first tries calling this method and then falls back to looking at the layout of the virtual environment on the filesystem if this method returns `None`?

---

_@AlexWaygood approved on 2025-06-26 15:10_

Thank you, this is so much better

---

_@AlexWaygood reviewed on 2025-06-26 15:10_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/analyze_graph.rs`:570 on 2025-06-26 15:10_

yeah I agree, it seems nicer this way. More concise but no less information.

---

_@MichaReiser reviewed on 2025-06-26 15:11_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:215 on 2025-06-26 15:11_

> which first tries calling this method and then falls back to looking at the layout of the virtual environment on the filesystem if this method returns None?

It no longer does the *first part*. It only looks at the layout

---

_@AlexWaygood reviewed on 2025-06-26 15:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:215 on 2025-06-26 15:17_

Ah I missed that! In that case, I'd possibly rename this one to `python_version_from_metadata`, and also rename `try_resolve_installation_python_version` to `python_version_from_layout`?

---

_@MichaReiser reviewed on 2025-06-26 15:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:215 on 2025-06-26 15:47_

Sounds good, I can do that

---

_@AlexWaygood approved on 2025-06-26 15:55_

---

_Merged by @MichaReiser on 2025-06-26 15:57_

---

_Closed by @MichaReiser on 2025-06-26 15:57_

---

_Branch deleted on 2025-06-26 15:57_

---
