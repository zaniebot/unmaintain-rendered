```yaml
number: 15631
title: "Change `EnvironmentOptions::venv-path` to `Option<SystemPathBuf>`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/options-venv-path
created_at: 2025-01-21T09:32:52Z
updated_at: 2025-01-21T14:11:45Z
url: https://github.com/astral-sh/ruff/pull/15631
synced_at: 2026-01-10T20:05:43Z
```

# Change `EnvironmentOptions::venv-path` to `Option<SystemPathBuf>`

---

_Pull request opened by @MichaReiser on 2025-01-21 09:32_

## Summary

The `Options` struct is intended to capture the user's configuration options but
`EnvironmentOptions::venv_path` supports both a `SitePackages::Known` and `SitePackages::Derived`.

Users should only be able to provide `SitePackages::Derived`—they specify a path to a venv, and Red Knot derives the path to the site-packages directory. We'll only use the `Known` variant once we automatically discover the Python installation. 

That's why this PR changes `EnvironmentOptions::venv_path` from `Option<SitePackages>` to `Option<SystemPathBuf>`.

This requires making some changes to the file watcher test, and I decided to use `extra_paths` over venv path
because our venv validation is annoyingly correct -- making mocking a venv rather involved. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-01-21 09:32_

---

_Marked ready for review by @MichaReiser on 2025-01-21 09:39_

---

_Review requested from @carljm by @MichaReiser on 2025-01-21 09:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-21 09:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-21 09:39_

---

_Comment by @github-actions[bot] on 2025-01-21 09:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp approved on 2025-01-21 09:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:226 on 2025-01-21 12:11_

not sure I understand this TODO

---

_@AlexWaygood approved on 2025-01-21 12:11_

---

_@MichaReiser reviewed on 2025-01-21 12:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:226 on 2025-01-21 12:18_

The idea here would be that we warn if `program.python_version` isn't compatible with the venv's python because that suggests a configuration error.

---

_@AlexWaygood reviewed on 2025-01-21 12:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:226 on 2025-01-21 12:21_

I don't think that's necessarily true. People often use a newer Python version for local development than they use in production. That would result in the project's `requires-python` being set to e.g. `">= 3.9"` even though they're using Python 3.13 locally. We'd type-check assuming Python 3.9 (the version they're using in production), unless they explicitly override that on the command line

---

_@MichaReiser reviewed on 2025-01-21 12:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:226 on 2025-01-21 12:24_

With incompatible I mean if your python version is older than what's specified in requires python. Using a newer version isn't incompatible with requires python.

Or we warn if you set an explicit target version but your venv version doesn't match that version (it's not the same major/minor)

Both these situations suggest that there's something wrong with the setup and seems worth warning about.



---

_@AlexWaygood reviewed on 2025-01-21 12:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:226 on 2025-01-21 12:27_

I still worry that these could lead to noisy warnings that won't be useful to most users... but we can play it by ear and modulate it depending on the feedback we get from users, I suppose.

Could you make the TODO a bit more expansive so it's clear what we might want to warn about in the future?

---

_Merged by @MichaReiser on 2025-01-21 14:10_

---

_Closed by @MichaReiser on 2025-01-21 14:10_

---

_Branch deleted on 2025-01-21 14:10_

---
