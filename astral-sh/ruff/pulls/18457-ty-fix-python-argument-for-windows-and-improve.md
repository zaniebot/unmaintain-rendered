```yaml
number: 18457
title: "[ty] Fix `--python` argument for Windows, and improve error messages for bad `--python` arguments"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: alex/fewer-python-path-variants
created_at: 2025-06-04T11:00:59Z
updated_at: 2025-06-11T10:46:22Z
url: https://github.com/astral-sh/ruff/pull/18457
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Fix `--python` argument for Windows, and improve error messages for bad `--python` arguments

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/556.

On Windows, system installations have different layouts to virtual environments. In Windows virtual environments, the Python executable is found at `<sys.prefix>/Scripts/python.exe`. But in Windows system installations, the Python executable is found at `<sys.prefix>/python.exe`. That means that Windows users were able to point to Python executables inside virtual environments with the `--python` flag, but they weren't able to point to Python executables inside system installations.

This PR fixes that issue. It also makes a couple of other changes:
- Nearly all `sys.prefix` resolution is moved inside `site_packages.rs`. That was the original design of the `site-packages` resolution logic, but features implemented since the initial implementation have added some resolution and validation to `resolver.rs` inside the module resolver. That means that we've ended up with a somewhat confusing code structure and a situation where several checks are unnecessarily duplicated between the two modules.
- I noticed that we had quite bad error messages if you e.g. pointed to a path that didn't exist on disk with `--python` (we just gave a somewhat impenetrable message saying that we "failed to canonicalize" the path). I improved the error messages here and added CLI tests for `--python` and the `environment.python` configuration setting.

## Test Plan

- Existing tests pass
- Added new CLI tests
- I manually checked that virtual-environment discovery still works if no configuration is given

I'd love it if somebody who has a Windows machine setup for development could do some manual testing to check that pointing `--python` to a system-installation executable now works.

---

_Label `ty` added by @AlexWaygood on 2025-06-04 11:01_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-04 11:01_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-04 11:01_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-04 11:01_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-04 11:01_

---

_@AlexWaygood reviewed on 2025-06-04 11:03_

---

_Review comment by @AlexWaygood on `crates/ty/tests/cli.rs`:1055 on 2025-06-04 11:03_

this TODO can be resolved by propagating whether the `python` setting was obtained from a CLI flag or from a configuration file, and adding a new `SysPrefixPathOrigin` variant that captures this information. For now I'm deferring this to a followup.

---

_Comment by @github-actions[bot] on 2025-06-04 11:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-04 11:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @carljm removed by @carljm on 2025-06-04 14:57_

---

_Review comment by @MichaReiser on `crates/ty/tests/cli.rs`:1055 on 2025-06-05 06:59_

Should we create a help wanted issue for this?

---

_@MichaReiser approved on 2025-06-05 07:01_

Nice. Thanks for fixing this and improving the diagnostics and documentatin along the way

---

_@AlexWaygood reviewed on 2025-06-05 07:18_

---

_Review comment by @AlexWaygood on `crates/ty/tests/cli.rs`:1055 on 2025-06-05 07:18_

I actually already have the fix locally — it was originally part of the same patch, but I split it out from this PR as I realised it wasn't directly required to fix the bug here and the patch was starting to sprawl over too much code :P

---

_Merged by @AlexWaygood on 2025-06-05 07:19_

---

_Closed by @AlexWaygood on 2025-06-05 07:19_

---

_Branch deleted on 2025-06-05 07:19_

---

_Label `cli` added by @dhruvmanila on 2025-06-11 10:46_

---
