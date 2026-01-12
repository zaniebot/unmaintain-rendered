```yaml
number: 13586
title: "Rename the `ruff_vendored` crate to `red_knot_vendored`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/rename-ruff-vendored
created_at: 2024-10-01T10:21:08Z
updated_at: 2024-10-01T15:17:01Z
url: https://github.com/astral-sh/ruff/pull/13586
synced_at: 2026-01-12T15:55:44Z
```

# Rename the `ruff_vendored` crate to `red_knot_vendored`

---

_@AlexWaygood_

## Summary

Following #13436, @carljm and I are no longer getting automated review requests for typeshed-sync PRs: @dhruvmanila had to manually request my review on #13578. That's because our CODEOWNERs file only has us as "owning" paths that have `red_knot` in them:

https://github.com/astral-sh/ruff/blob/2a36b47f1395284d32ae2015e6c626f242922224/.github/CODEOWNERS#L19-L21

We could just add another line to CODEOWNERS, like we already have for `ruff_db`, but renaming the crate has other advantages too:
- This crate really is red-knot-specific. If the Ruff linter or formatter decides it needs to vendor some other files in the future, it won't use this crate to do it, since this crate packages all of typeshed into a zip at build time, which is entirely unnecessary for the linter and formatter
- It's pretty minor, but it's nice to have all the red-knot-related crates next to each other in the folder list when I'm scrolling through them in my editor.

## Test Plan

- `cargo test`
- Grepped the source code to check there weren't any remaining references to `ruff_vendored` anywhere.


---

_Label `red-knot` added by @AlexWaygood on 2024-10-01 10:21_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-01 10:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-01 10:21_

---

_Comment by @github-actions[bot] on 2024-10-01 10:39_

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

_@dhruvmanila approved on 2024-10-01 13:09_

Looks fine to me. Unrelated to this PR, do we need the `vendor` directory inside the `red_knot_vendored` crate or should we move the `typeshed` at the top-level? The double "vendor" was confusing at first but it's fine regardless.

---

_Comment by @AlexWaygood on 2024-10-01 13:17_

> Unrelated to this PR, do we need the `vendor` directory inside the `red_knot_vendored` crate or should we move the `typeshed` at the top-level? The double "vendor" was confusing at first but it's fine regardless.

Yeah, I suppose having the `vendor/` directory made more sense when these vendored files were part of a bigger crate like `red_knot_module_resolver` or `red_knot_python_semantic`; in those situations, it was useful to separate the "vendored data" from the actual Rust code. But now that they're in their own crate, there's very little actual Rust code 😆

I feel like I still weakly prefer having them in a subdirectory for that reason, but definitely don't have a strong opinion

---

_Comment by @MichaReiser on 2024-10-01 14:52_

> This crate really is red-knot-specific. If the Ruff linter or formatter decides it needs to vendor some other files in the future, it won't use this crate to do it, since this crate packages all of typeshed into a zip at build time, which is entirely unnecessary for the linter and formatter

I don't think I agree with this. Ruff analyze might need the same vendored files if it wants to detect    standard library or third party dependencies. 

I used this name because ruff will use this crate long term. Although that can be said for an red knot module. 

TLDR: I don't mind this change. 

---

_Comment by @AlexWaygood on 2024-10-01 15:16_

> Ruff analyze might need the same vendored files if it wants to detect standard library or third party dependencies.

Well, `ruff_graph` already depends on the `red_knot_python_semantic` crate, so having another `red_knot` crate in its dependencies feels like it doesn't hurt that much ;) And the API this crate exposes isn't really usable right now unless you also depend on `red_knot_python_semantic`.

I feel like we can consider renaming them again when Ruff and red-knot truly merge, but for now it feels very much like part of the red-knot project to me. And renaming is fairly cheap!

---

_Merged by @AlexWaygood on 2024-10-01 15:16_

---

_Closed by @AlexWaygood on 2024-10-01 15:17_

---

_Branch deleted on 2024-10-01 15:17_

---
