```yaml
number: 12731
title: "[red-knot] Cleanups to logic resolving `site-packages` from a venv path"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/venvs
created_at: 2024-08-07T14:08:45Z
updated_at: 2024-08-07T14:48:17Z
url: https://github.com/astral-sh/ruff/pull/12731
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Cleanups to logic resolving `site-packages` from a venv path

---

_@AlexWaygood_

## Summary

This PR is a followup to #12716. It does the following things:
- Addresses @MichaReiser's review comment from https://github.com/astral-sh/ruff/pull/12716#discussion_r1706014771, using `if cfg!()` within a function instead of having two separate functions that have `#[cfg()]` attributes on them.
- Deletes all the activation scripts, and the `.gitignore` file, from the virtual environment in `resources/test`. These aren't needed for our purposes. While I'd prefer to make the minimum possible changes to the test virtual environment we have committed there, some followup changes I'll be making will require committing more test virtual environments, and it's silly for them all to be so big.
- Renames the committed virtual environment in `resources/test` from `empty-unix-venv` to `unix-uv-venv`. It turns out there are small differences in virtual environments depending on which tool created the virtual environment, so it's useful for that to be reflected in the name.

## Test Plan

`cargo test -p red_knot_workspace`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-07 14:08_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-07 14:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-07 14:08_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/site_packages.rs`:136 on 2024-08-07 14:22_

```suggestion
    #[cfg_attr(ignore="Windows has a different venv layout", not(target_os = "windows"))]
```

I think it should then always compile the code (which is nice when doing refactoring) and even explicitly emit a message why the test is ignored on windows.

---

_@MichaReiser approved on 2024-08-07 14:22_

---

_Comment by @github-actions[bot] on 2024-08-07 14:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-08-07 14:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/site_packages.rs`:136 on 2024-08-07 14:26_

Ah, clever!

---

_Merged by @AlexWaygood on 2024-08-07 14:48_

---

_Closed by @AlexWaygood on 2024-08-07 14:48_

---

_Branch deleted on 2024-08-07 14:48_

---
