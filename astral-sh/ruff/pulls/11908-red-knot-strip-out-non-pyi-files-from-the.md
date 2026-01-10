```yaml
number: 11908
title: "[red-knot] Strip out non-pyi files from the typeshed zip file created at build time"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: typeshed-validation
created_at: 2024-06-17T17:34:05Z
updated_at: 2024-06-18T13:37:27Z
url: https://github.com/astral-sh/ruff/pull/11908
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Strip out non-pyi files from the typeshed zip file created at build time

---

_Pull request opened by @AlexWaygood on 2024-06-17 17:34_

## Summary

This tweaks the red-knot `build.rs` script to ensure that all non-`.pyi` files are stripped out from the zip created at build time, and asserts as much in our test suite. This means there's less room for error if typeshed changes its directory structure unexpectedly, which could theoretically cause us to accidentally start bundling parts of typeshed's test suite as part of the zip.

The only non-`.pyi` file we'll need from typeshed is the `stdlib/VERSIONS` file, and I think it's easier if we add that to the Ruff binary separately via `include_str!()`.

## Test Plan

Add some more assertions to `module.rs`.


---

_Label `red-knot` added by @AlexWaygood on 2024-06-17 17:34_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-17 17:34_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-17 17:34_

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:91 on 2024-06-17 21:33_

Doesn't the walkdir entry expose whether it is a directory or not? I find it a bit confusing that we are handling dirs in this predicate 

---

_Review comment by @MichaReiser on `crates/red_knot/build.rs`:40 on 2024-06-17 21:34_

What's the motivation for passing a relative path when the predicate logic makes the path absolute again by joining (and the extension part doesn't care about relative or absolute)

---

_@MichaReiser reviewed on 2024-06-17 21:34_

---

_Comment by @carljm on 2024-06-17 23:07_

I don't have any major objection to this PR, but I find the motivation for it a little weak, and it's not clear to me that it's worth the added build-script complexity.

If typeshed changes its directory structure, we should catch that in the typeshed-vendoring-update PR (and we should have tests relying on typeshed that will fail in that PR to alert us to the problem.)

---

_@carljm reviewed on 2024-06-17 23:09_

If this was motivated by an actual problem we encountered that this would have solved, then I don't have a problem with it.

---

_Comment by @AlexWaygood on 2024-06-18 13:37_

I think you're right, it's probably not worth the added complexity for now. Thanks for the reviews.

---

_Closed by @AlexWaygood on 2024-06-18 13:37_

---

_Branch deleted on 2024-06-18 13:37_

---
