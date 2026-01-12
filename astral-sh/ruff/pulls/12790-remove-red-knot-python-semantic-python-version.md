```yaml
number: 12790
title: "Remove `red_knot_python_semantic::python_version::TargetVersion`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/remove-target-version
created_at: 2024-08-09T19:18:10Z
updated_at: 2024-08-10T13:28:32Z
url: https://github.com/astral-sh/ruff/pull/12790
synced_at: 2026-01-12T15:55:42Z
```

# Remove `red_knot_python_semantic::python_version::TargetVersion`

---

_@AlexWaygood_

## Summary

This PR is a followup to @MichaReiser's comment at https://github.com/astral-sh/ruff/pull/12782#discussion_r1711701881:

> I think it's now a bit confusing when to use `TargetVersion` vs `PythonVersion`? Shouldn't all (or at least most) our code be open for new Python versions? Or to phrase it differently, what's the benefit of limiting `TargetVersion` to a fixed set?

Instead of having two types in `ruff_python_semantic` -- one that represents an arbitrary Python version, and another that represents the fixed set of versions that we know we support -- this PR consolidates it so that we only use a single type (the more general one). The only case where we need a fixed enumeration of versions that we actually support (at least currently) is in a CLI argparsing context -- but the CLI crates already have their own enums.

This will have quite a few merge conflicts with https://github.com/astral-sh/ruff/pull/12786... I'm okay with waiting until @MichaReiser's stack of module-resolver validation PRs has landed before merging this. It's not urgent.

<!-- How was it tested? -->

`cargo test`

---

_Label `red-knot` added by @AlexWaygood on 2024-08-09 19:18_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-09 19:18_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-09 19:18_

---

_Comment by @github-actions[bot] on 2024-08-09 19:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-08-09 19:53_

Looks fine to me!

---

_@MichaReiser approved on 2024-08-10 13:27_

Thanks. Feel free to go ahead with merging. 

---

_Merged by @AlexWaygood on 2024-08-10 13:28_

---

_Closed by @AlexWaygood on 2024-08-10 13:28_

---

_Branch deleted on 2024-08-10 13:28_

---
