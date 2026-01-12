```yaml
number: 11826
title: "red-knot: Add a method to resolve a file for an arbitrary `VfsPath`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-refactor-vfs-file-methods
created_at: 2024-06-10T17:21:29Z
updated_at: 2024-06-18T12:13:20Z
url: https://github.com/astral-sh/ruff/pull/11826
synced_at: 2026-01-12T15:55:39Z
```

# red-knot: Add a method to resolve a file for an arbitrary `VfsPath`

---

_@MichaReiser_

## Summary

When I started working on porting the module resolver, I realized that the `path_to_module` function operates on an arbitrary `VfsPath` and it must be able to resolve it to a `VfsFile`. 

The problem I ran into is that the existing salsa db doesn't provide an API to resolve the file for an arbitrary path. This PR addresses this by replacing `db.file` and `db.vendored` with `vendored_path_to_file`, `file_system_path_to_file`, `vfs_path_to_file` functions.

The advantage of free-standing functions over trait functions is that these can take an `impl AsRef<.Path>` as argument which makes them more ergonomic. I kept the path specific operations because they're slightly more ergonomic (they don't require creating a `VfsPath` first).

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-10 17:21_

---

_Label `red-knot` added by @MichaReiser on 2024-06-10 17:21_

---

_Review requested from @carljm by @MichaReiser on 2024-06-10 18:40_

---

_Renamed from "red-knot: Add a method to resolve a file for an arbitrary `VfsPath`" to "red-knot[salsa part 5]: Add a method to resolve a file for an arbitrary `VfsPath`" by @MichaReiser on 2024-06-11 14:24_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-06-11 14:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-11 14:26_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:18 on 2024-06-12 17:31_

I would consider naming this function `system_path_to_file`. It's more concise, and IMO having `file` twice in the name is a little confusing.

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:24 on 2024-06-12 17:34_

I don't really understand this sentence :(

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:31 on 2024-06-12 17:37_

```suggestion
/// Interns a vendored file path. Returns `Some(file)` if the vendored file exists, else `None`.
```

---

_@AlexWaygood reviewed on 2024-06-12 17:38_

---

_@MichaReiser reviewed on 2024-06-13 07:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vfs.rs`:24 on 2024-06-13 07:53_

I rewrote the sentence

---

_@AlexWaygood reviewed on 2024-06-13 07:57_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:24 on 2024-06-13 07:57_

Thank you! That's much clearer now 

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vfs.rs`:23 on 2024-06-13 07:57_

```suggestion
    // that file. This function filters out files that don't exist, but Salsa will know that it must
```

---

_@AlexWaygood approved on 2024-06-13 07:57_

---

_Comment by @github-actions[bot] on 2024-06-13 08:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "red-knot[salsa part 5]: Add a method to resolve a file for an arbitrary `VfsPath`" to "red-knot: Add a method to resolve a file for an arbitrary `VfsPath`" by @MichaReiser on 2024-06-18 11:59_

---

_Merged by @MichaReiser on 2024-06-18 12:03_

---

_Closed by @MichaReiser on 2024-06-18 12:03_

---

_Branch deleted on 2024-06-18 12:03_

---
