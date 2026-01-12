```yaml
number: 13436
title: Use an empty vendored file system in Ruff
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/ruff-empty-vendored-fs
created_at: 2024-09-21T12:35:34Z
updated_at: 2024-09-21T16:45:42Z
url: https://github.com/astral-sh/ruff/pull/13436
synced_at: 2026-01-12T15:55:44Z
```

# Use an empty vendored file system in Ruff

---

_@MichaReiser_

## Summary

This PR changes removes the typeshed stubs from the vendored file system shipped with ruff
and instead ships an empty "typeshed".

Making the typeshed files optional required extracting the typshed files into a new `ruff_vendored` crate. I do like this even if all our builds always include typeshed because it means `red_knot_python_semantic` contains less code that needs compiling.

This also allows us to use deflate because the compression algorithm doesn't matter for an archive containing a single, empty file. 

## Test Plan

`cargo test`

I verified with ` cargo tree -f "{p} {f}" -p <package> ` that:

* red_knot_wasm: enables `deflate` compression
* red_knot: enables `zstd` compression
* `ruff`: uses stored


I'm not quiet sure how to build the binary that maturin builds but comparing the release artifact size with `strip = true` shows a `1.5MB` size reduction

---

_Comment by @MichaReiser on 2024-09-21 12:51_

@charliermarsh this is a potential short term fix for the conda forge build

---

_Comment by @github-actions[bot] on 2024-09-21 13:32_

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

_Marked ready for review by @MichaReiser on 2024-09-21 13:42_

---

_Review requested from @carljm by @MichaReiser on 2024-09-21 13:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-21 13:42_

---

_@charliermarsh approved on 2024-09-21 13:48_

---

_Comment by @charliermarsh on 2024-09-21 13:49_

This appears to be working: https://github.com/conda-forge/ruff-feedstock/pull/220

---

_Comment by @charliermarsh on 2024-09-21 13:49_

I'm happy to release it today if you want

---

_Comment by @MichaReiser on 2024-09-21 14:08_

Nice. Could you try running the new analyze method over a real project? Just to make sure we don't show annoying warnings when the versions file is empty :laughing: 

---

_Comment by @MichaReiser on 2024-09-21 14:38_

But yeah. Feel free to merge and release the changes to unblock conda forge.

---

_Label `internal` added by @MichaReiser on 2024-09-21 14:40_

---

_Comment by @MichaReiser on 2024-09-21 14:40_

For changelog: Fix conda forge build.

---

_Comment by @MichaReiser on 2024-09-21 15:15_

> This appears to be working: [conda-forge/ruff-feedstock#220](https://github.com/conda-forge/ruff-feedstock/pull/220)

I love your confidence :laughing: 

---

_@AlexWaygood reviewed on 2024-09-21 16:14_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/.vendored.rs.pending-snap`:1 on 2024-09-21 16:14_

This file shouldn't be here

---

_Comment by @charliermarsh on 2024-09-21 16:27_

Just ran, no warnings :)

---

_Merged by @charliermarsh on 2024-09-21 16:31_

---

_Closed by @charliermarsh on 2024-09-21 16:31_

---

_Branch deleted on 2024-09-21 16:31_

---

_@MichaReiser reviewed on 2024-09-21 16:34_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/.vendored.rs.pending-snap`:1 on 2024-09-21 16:34_

Thanks. I don't understand why it sometimes creates those files but doesn't delete them when the test is done.

---
