```yaml
number: 11825
title: "red-knot: Add directory support to `MemoryFileSystem`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-memory-fs-directories
created_at: 2024-06-10T14:44:47Z
updated_at: 2024-06-13T07:58:42Z
url: https://github.com/astral-sh/ruff/pull/11825
synced_at: 2026-01-12T15:55:39Z
```

# red-knot: Add directory support to `MemoryFileSystem`

---

_@MichaReiser_

## Summary

This PR adds support for handling directories to the `MemoryFileSystem`. 
I'm adding this now because the module resolver contains logic that branches if `path` is a directory.

I used this PR to remove permissions support from the `MemoryFileSystem`. I think having support for it is misleading, it gives the impression that the file system would check permissions when reading/writing which it does not and I don't plan on adding.
The `MemoryFileSystem` is mainly intended for testing. We should use the real file system to test more advanced or even platform specific filesystem behavior. That's the only way to get that right. 

## Test Plan

Added tests


---

_Review requested from @dhruvmanila by @MichaReiser on 2024-06-10 14:44_

---

_Review requested from @carljm by @MichaReiser on 2024-06-10 14:44_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-06-10 14:44_

---

_Label `red-knot` added by @MichaReiser on 2024-06-10 14:45_

---

_Closed by @MichaReiser on 2024-06-11 13:55_

---

_Renamed from "red-knot: Add directory support to `MemoryFileSystem`" to "red-knot[salsa part 4]: Add directory support to `MemoryFileSystem`" by @MichaReiser on 2024-06-11 14:21_

---

_Reopened by @MichaReiser on 2024-06-11 14:21_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-06-11 14:22_

---

_Comment by @github-actions[bot] on 2024-06-11 14:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/ruff_db/src/file_system/memory.rs`:19 on 2024-06-12 14:04_

+1 to this approach. We should use in-memory file system for fast and easy testing of semantic analysis, not for testing filesystem behaviors.

---

_Review comment by @carljm on `crates/ruff_db/src/file_system/memory.rs`:172 on 2024-06-12 14:06_

Why the switch from `FxDashMap` to an `FxHashMap` wrapped in a `RwLock`?

---

_Review comment by @carljm on `crates/ruff_db/src/file_system/memory.rs`:216 on 2024-06-12 14:08_

I am very surprised that we have to implement such a thing ourselves instead of just using a battle-tested library method for this.

---

_@carljm approved on 2024-06-12 14:12_

Looks like there are some clippy/test problems to resolve before merging this.

---

_@MichaReiser reviewed on 2024-06-12 14:48_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/memory.rs`:172 on 2024-06-12 14:48_

Good question. I don't think it was necessary as part of this PR. I first thought I would need a `BTreeMap`. I'll leave it this way because I do need a `BTreeMap` when adding support for removing directories.

---

_@MichaReiser reviewed on 2024-06-12 14:49_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/file_system/memory.rs`:216 on 2024-06-12 14:49_

I think it exists, it's just not stabalized. @BurntSushi knows more ;)

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/memory.rs`:93 on 2024-06-12 16:55_

```suggestion
    /// Sets the last modified timestamp of the file stored at `path` to now.
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/file_system/memory.rs`:223 on 2024-06-12 17:13_

```suggestion
        if let Some(c @ (camino::Utf8Component::Prefix(..) | camino::Utf8Component::RootDir)) =
```

---

_@AlexWaygood approved on 2024-06-12 17:15_

---

_Renamed from "red-knot[salsa part 4]: Add directory support to `MemoryFileSystem`" to "red-knot: Add directory support to `MemoryFileSystem`" by @MichaReiser on 2024-06-13 07:45_

---

_Merged by @MichaReiser on 2024-06-13 07:48_

---

_Closed by @MichaReiser on 2024-06-13 07:48_

---

_Branch deleted on 2024-06-13 07:48_

---

_Comment by @codspeed-hq[bot] on 2024-06-13 07:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/salsa-memory-fs-directories)

### Merging #11825 will **improve performances by 13.32%**

<sub>Comparing <code>salsa-memory-fs-directories</code> (671d556) with <code>main</code> (d4dd96d)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `salsa-memory-fs-directories` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 915.2 µs | 807.6 µs | +13.32% |


---
