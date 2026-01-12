```yaml
number: 18937
title: "[ty] Move search path resolution to `Options::to_program_settings`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/move-search-path-resolution
created_at: 2025-06-25T13:26:17Z
updated_at: 2025-06-25T16:00:40Z
url: https://github.com/astral-sh/ruff/pull/18937
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Move search path resolution to `Options::to_program_settings`

---

_@MichaReiser_

## Summary

This PR moves the `SearchPaths::from_settings` call and the resolution of the final Python version into `Options::to_program_settings`. 

We want to use `ProgramSettings` as a cache key for persistent caching (e.g., checking different Python versions should resolve in loading different persistent caches). For this to work well, it's important that `ProgramSettings` mainly holds resolved settings. For example, it's essential that the following two runs in an activated virtual environment using Python 3.10 result in the same computed cache key: `ty check --python-version 3.10` and `ty check`.

The reason I made this change now is that I investigated what it would mean if the LSP passed the selected interpreter in the Python extension as a lowest-priority fallback value. We want to make sure that ty reuses the persistent cache between a `ty check --python-version 3.10` run and when the interpreter selected in the IDE uses Python 3.10.

This should hopefully also simplify the handling of `VIRTUAL_ENV` and `CONDA_ENV` as fallback if no python version is provided.



## Test Plan

Existing tests. I did move some tests from `ProjectMetadata` to CLI tests because keeping them in the metadata tests would have required increasing the visibility of many fields.

---

_Renamed from "[ty] Move search path resolution to `Options::to_program_settings`"" to "[ty] Move search path resolution to `Options::to_program_settings`" by @MichaReiser on 2025-06-25 13:26_

---

_Label `internal` added by @MichaReiser on 2025-06-25 13:26_

---

_Label `ty` added by @MichaReiser on 2025-06-25 13:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-25 13:26_

---

_Comment by @github-actions[bot] on 2025-06-25 13:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-25 13:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:128 on 2025-06-25 13:40_

I'm not sure if there's a test demonstrating why this is needed. The `Default`, `Config` and `Cli` cases are inherent by how we resolve options. I would expect that the other cases work because we simply re-run the entire settings resolution (including the search path discovery) whenever the settings change and the priority should be encoded in the setting/search path resolution.

---

_@MichaReiser reviewed on 2025-06-25 13:40_

---

_@AlexWaygood reviewed on 2025-06-25 13:44_

This looks great to me!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:128 on 2025-06-25 13:51_

I think I was possibly confused about when exactly the `update_search_paths` method is called.

Could it be called without re-resolving all other settings if e.g. a `.pth` file in the `site-packages` directory changes (which affects editable-installation search paths)? If so, it's possible that the `update_search_paths` method might accidentally override the Python version specified in a pyproject.toml file, even though the version in the pyproject.toml file should always take precedence over the Python version inferred from a virtual environment?

---

_@AlexWaygood reviewed on 2025-06-25 13:51_

---

_@MichaReiser reviewed on 2025-06-25 13:54_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:128 on 2025-06-25 13:54_

It's only called when the VERSIONS file in the typeshed directory changed and we then resolve all settings the same as we would when the configuration changed. 

---

_Marked ready for review by @MichaReiser on 2025-06-25 13:54_

---

_Review requested from @carljm by @MichaReiser on 2025-06-25 13:54_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-25 13:54_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-25 13:54_

---

_@AlexWaygood approved on 2025-06-25 14:16_

I haven't checked line-by-line but the change makes sense to me, and nothing looked obviously wrong from skimming through. I also think we've got pretty good test coverage for all this, which gives me confidence in the refactor!

---

_Merged by @MichaReiser on 2025-06-25 16:00_

---

_Closed by @MichaReiser on 2025-06-25 16:00_

---

_Branch deleted on 2025-06-25 16:00_

---
