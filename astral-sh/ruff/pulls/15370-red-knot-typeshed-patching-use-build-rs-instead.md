```yaml
number: 15370
title: "[red-knot] Typeshed patching: use build.rs instead of workflow"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/typeshed_sync-copy-instead-of-symlink
created_at: 2025-01-09T09:33:04Z
updated_at: 2025-01-09T12:20:14Z
url: https://github.com/astral-sh/ruff/pull/15370
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Typeshed patching: use build.rs instead of workflow

---

_@sharkdp_

## Summary

The symlink-approach in the typeshed-sync workflow caused some [problems on Windows](https://github.com/astral-sh/ruff/pull/15138#issuecomment-2578642129), even though it seemed to work fine in CI.

Here, we rely on `build.rs` to patch typeshed instead, which allows us to get rid of the modifications in the workflow (thank you @MichaReiser).

## Test Plan

- Made sure that changes to `knot_extensions.pyi` result in a recompile of `red_knot_vendored`.

---

_Label `internal` added by @sharkdp on 2025-01-09 09:33_

---

_Label `red-knot` added by @sharkdp on 2025-01-09 09:33_

---

_Review requested from @carljm by @sharkdp on 2025-01-09 09:33_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-09 09:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-09 09:33_

---

_Review comment by @MichaReiser on `.github/workflows/sync_typeshed.yaml`:61 on 2025-01-09 09:39_

I don't mind the approach taken here. An alternative could be to copy the file in the `red_knot_vendored`'s `build.rs`.

---

_@MichaReiser reviewed on 2025-01-09 09:39_

---

_@sharkdp reviewed on 2025-01-09 09:57_

---

_Review comment by @sharkdp on `.github/workflows/sync_typeshed.yaml`:61 on 2025-01-09 09:57_

Ohhh, I like this much more. Thanks!

---

_@sharkdp reviewed on 2025-01-09 09:59_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/build.rs`:72 on 2025-01-09 09:59_

Removing this means that we will rebuild `red_knot_vendored` if *any* file in the package has changed ([ref](https://doc.rust-lang.org/cargo/reference/build-scripts.html#change-detection)). I need this, because `knot_extensions.pyi` is not in the `vendor/typeshed` directory, and we need to pick up changes to it.

And I don't think there's a huge drawback since there are no files in `red_knot_vendored` that should *not* trigger a rebuild except for the README.

---

_Comment by @github-actions[bot] on 2025-01-09 10:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-09 10:40_

Neat

---

_Renamed from "[red-knot] Typeshed sync: use copy instead of symlink" to "[red-knot] Typeshed patching: use build.rs instead of workflow" by @sharkdp on 2025-01-09 10:48_

---

_Merged by @sharkdp on 2025-01-09 10:50_

---

_Closed by @sharkdp on 2025-01-09 10:50_

---

_Branch deleted on 2025-01-09 10:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_vendored/build.rs`:67 on 2025-01-09 11:28_

When I originally wrote this function, I intended to write it so that it could be used to zip up any directory into a zip archive -- that's why it's called `zip_dir()`, and that's why it accepts the typeshed directory path as a parameter rather than hardcoding it in the function. I think since I originally wrote it, it's already grown a few features that make it quite specific to its use-case here, such as the conditional logic a few lines above for deciding what the compression method should be. So I don't really mind much hardcoding things like `"stdlib/VERSIONS"` directly into the function body. But at this stage, it probably doesn't make much sense for the typeshed directory to be passed into the function as a parameter, and we should probably change the name of the function 😄

---

_@AlexWaygood reviewed on 2025-01-09 11:28_

Ahh, using `build.rs` for this is so much cleaner!

---

_@sharkdp reviewed on 2025-01-09 12:20_

---

_Review comment by @sharkdp on `crates/red_knot_vendored/build.rs`:67 on 2025-01-09 12:20_

Sorry about that, I figured it would be okay to modify in this way since it wasn't being used anywhere else and not in a place where it could be imported from anywhere else. See https://github.com/astral-sh/ruff/pull/15372

---
