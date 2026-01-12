```yaml
number: 12215
title: "[red-knot] Only store absolute paths in `Files`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: files-use-absolute-paths
created_at: 2024-07-06T14:34:16Z
updated_at: 2024-07-09T07:52:15Z
url: https://github.com/astral-sh/ruff/pull/12215
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Only store absolute paths in `Files`

---

_@MichaReiser_

## Summary

This PR normalizes system paths in `Files` before looking them up (and storing) them. 

This prevents that we have multiple file ingredients that all point to the same underlying path.

Fixes https://github.com/astral-sh/ruff/issues/11907
Fixes https://github.com/astral-sh/ruff/issues/12209

## Test Plan

I verified that changes to files correctly propagate when running the Red Knot CLI with relative paths


---

_Comment by @codspeed-hq[bot] on 2024-07-06 14:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/files-use-absolute-paths)

### Merging #12215 will **not alter performance**

<sub>Comparing <code>files-use-absolute-paths</code> (f3818a2) with <code>main</code> (3d3ff10)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Label `red-knot` added by @MichaReiser on 2024-07-07 09:46_

---

_Marked ready for review by @MichaReiser on 2024-07-07 09:59_

---

_Review requested from @carljm by @MichaReiser on 2024-07-07 09:59_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-07 09:59_

---

_Comment by @github-actions[bot] on 2024-07-07 10:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/path.rs`:385 on 2024-07-07 20:01_

Is it worth adding a comment similar to https://github.com/astral-sh/ruff/blob/757c75752e1a728669bb97a412ddf205d165b0ea/crates/ruff_db/src/vendored.rs#L260-L265 here?

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/system/test.rs`:81 on 2024-07-07 20:03_

What's the reason for the method on one variant using the `cwd` acronym, but the method on the other variant spelling it out as `current_directory()`?

---

_@AlexWaygood approved on 2024-07-07 20:04_

---

_@carljm approved on 2024-07-08 18:08_

---

_@MichaReiser reviewed on 2024-07-09 07:35_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:385 on 2024-07-09 07:35_

I added an example test case that also verifies that it works that way.

---

_@MichaReiser reviewed on 2024-07-09 07:36_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/test.rs`:81 on 2024-07-09 07:36_

No, that's not intentional. Thanks for catching it.

---

_Merged by @MichaReiser on 2024-07-09 07:52_

---

_Closed by @MichaReiser on 2024-07-09 07:52_

---

_Branch deleted on 2024-07-09 07:52_

---
