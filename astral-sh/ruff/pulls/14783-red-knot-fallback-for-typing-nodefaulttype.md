```yaml
number: 14783
title: "[red-knot] Fallback for `typing._NoDefaultType`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/no-default-type-fallback
created_at: 2024-12-05T08:00:10Z
updated_at: 2024-12-05T08:17:56Z
url: https://github.com/astral-sh/ruff/pull/14783
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Fallback for `typing._NoDefaultType`

---

_Pull request opened by @sharkdp on 2024-12-05 08:00_

## Summary

`typing_extensions` has a `>=3.13` re-export for the `typing.NoDefault` singleton, but not for `typing._NoDefaultType`. This causes problems as soon as we understand `sys.version_info` branches, so we explicity switch to `typing._NoDefaultType` for Python 3.13 and later.

This is a part of #14759 that I thought might make sense to break out and merge in isolation.

## Test Plan

New test that will become more meaningful with #12700 


---

_Label `red-knot` added by @sharkdp on 2024-12-05 08:00_

---

_Review requested from @carljm by @sharkdp on 2024-12-05 08:00_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-05 08:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-05 08:00_

---

_@MichaReiser reviewed on 2024-12-05 08:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1934 on 2024-12-05 08:04_

```suggestion
                if python_version >= PythonVersion::PY313 {
                    CoreStdlibModule::Typing
                } else {
                    CoreStdlibModule::TypingExtensions
                }
```

---

_@sharkdp reviewed on 2024-12-05 08:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1934 on 2024-12-05 08:06_

Oh, nice!

---

_Comment by @github-actions[bot] on 2024-12-05 08:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2024-12-05 08:17_

I'm merging this to decrease the diff in the statically-known branches PR. It seems relatively uncontroversial, but please let me know if something is off.

---

_Merged by @sharkdp on 2024-12-05 08:17_

---

_Closed by @sharkdp on 2024-12-05 08:17_

---

_Branch deleted on 2024-12-05 08:17_

---
