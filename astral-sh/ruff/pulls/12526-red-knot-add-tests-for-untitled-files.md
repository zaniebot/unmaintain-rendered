```yaml
number: 12526
title: "[red-knot] Add tests for untitled files"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: dhruv/untitled-files
head: dhruv/untitled-files-2
created_at: 2024-07-26T10:53:33Z
updated_at: 2024-07-26T12:20:50Z
url: https://github.com/astral-sh/ruff/pull/12526
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Add tests for untitled files

---

_Pull request opened by @dhruvmanila on 2024-07-26 10:53_

## Summary

This PR addresses the review comments in https://github.com/astral-sh/ruff/pull/12492.

It's a separate PR to better facilitate the review process.

> [!NOTE]
>
> This PR will be merged into the parent PR and then a single commit will be created on `main`.

## Test Plan

`cargo test -p ruff_db`


---

_Label `red-knot` added by @dhruvmanila on 2024-07-26 10:53_

---

_Review requested from @carljm by @dhruvmanila on 2024-07-26 10:53_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-26 10:53_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-07-26 10:53_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:160 on 2024-07-26 10:59_

I would change this method to return an `Option`, so we don't need the `try` method. 

It feels a bit funny that this operation can fail. I suspect that this will be somewhat awkward in the LSP, but let's see. 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:160 on 2024-07-26 11:01_

I think it would be best to not override any existing file. Overriding an existing file would mean that we suddenly have multiple files mapping to the same path. This could be come awkward to debug.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:625 on 2024-07-26 11:02_

I think the full string that VS code uses is `untitled://Untitled-1`. Can we add a test for this? 

---

_@MichaReiser reviewed on 2024-07-26 11:02_

---

_Comment by @github-actions[bot] on 2024-07-26 11:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-07-26 11:21_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/system/path.rs`:625 on 2024-07-26 11:21_

I just checked in VS Code and it's
* `untitled:Untitled-1`
* `vscode-notebook-cell:Untitled-1.ipynb`

I'll update the test.

---

_@MichaReiser reviewed on 2024-07-26 11:24_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/path.rs`:625 on 2024-07-26 11:24_

Interesting, https://github.com/neovim/neovim/issues/21276 suggests that it should be a URL with the format `untitled://`

---

_@dhruvmanila reviewed on 2024-07-26 11:31_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/files.rs`:160 on 2024-07-26 11:31_

I think the `try` method would still be required to fetch the file when trying to sync it.

---

_@MichaReiser reviewed on 2024-07-26 11:42_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:160 on 2024-07-26 11:42_

Hmm only  when going from `path` to `File`. Maybe we should remove that sync method for now and instead require calling `file.sync`.  That does mean that we can't offer a `write_virtual_system_file` test helper. I think that's fine for now.

---

_@dhruvmanila reviewed on 2024-07-26 11:54_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/files.rs`:160 on 2024-07-26 11:54_

Ok, I've removed the sync logic.

---

_@dhruvmanila reviewed on 2024-07-26 11:54_

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/system/path.rs`:625 on 2024-07-26 11:54_

I've added these test cases.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-26 11:55_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:314 on 2024-07-26 12:00_

Sorry. This sync function is okay where we already have a file (because it doesn't need looking up the file by path)

---

_@MichaReiser approved on 2024-07-26 12:01_

---

_Merged by @dhruvmanila on 2024-07-26 12:10_

---

_Closed by @dhruvmanila on 2024-07-26 12:10_

---

_Branch deleted on 2024-07-26 12:10_

---

_Comment by @codspeed-hq[bot] on 2024-07-26 12:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/untitled-files-2)

### Merging #12526 will **not alter performance**

<sub>Comparing <code>dhruv/untitled-files-2</code> (d7b915e) with <code>main</code> (71f7aa4)</sub>



### Summary

`✅ 30` untouched benchmarks

`🆕 3` new benchmarks
`⁉️ 3` dropped benchmarks


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/untitled-files-2)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/untitled-files-2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⁉️ | `red_knot_check_file[cold]` | 14 ms | N/A | N/A |
| ⁉️ | `red_knot_check_file[incremental]` | 446.5 µs | N/A | N/A |
| ⁉️ | `red_knot_check_file[without_parse]` | 6.5 ms | N/A | N/A |
| 🆕 | `red_knot_check_file[cold]` | N/A | 13.8 ms | N/A |
| 🆕 | `red_knot_check_file[incremental]` | N/A | 438.8 µs | N/A |
| 🆕 | `red_knot_check_file[without_parse]` | N/A | 6.4 ms | N/A |


---
