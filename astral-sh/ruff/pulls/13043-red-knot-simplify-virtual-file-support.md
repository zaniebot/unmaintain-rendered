```yaml
number: 13043
title: "[red-knot] Simplify virtual file support"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/virtual-file
created_at: 2024-08-22T06:58:17Z
updated_at: 2024-08-23T07:13:21Z
url: https://github.com/astral-sh/ruff/pull/13043
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Simplify virtual file support

---

_Pull request opened by @dhruvmanila on 2024-08-22 06:58_

## Summary

This PR simplifies the virtual file support in the red knot core, specifically:

* Update `File::add_virtual_file` method to `File::virtual_file` which will always create a new virtual file and override the existing entry in the lookup table
* Add `VirtualFile` which is a wrapper around `File` and provides methods to increment the file revision / close the virtual file
* Add a new `File::try_virtual_file` to lookup the `VirtualFile` from `Files`
* Add `File::sync_virtual_path` which takes in the `SystemVirtualPath`, looks up the `VirtualFile` for it and calls the `sync` method to increment the file revision
* Removes the `virtual_path_metadata` method on `System` trait

## Test Plan

- [x] Make sure the existing red knot tests pass
- [x] Updated code works well with the LSP


---

_Label `red-knot` added by @dhruvmanila on 2024-08-22 06:58_

---

_Comment by @codspeed-hq[bot] on 2024-08-22 07:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/virtual-file)

### Merging #13043 will **not alter performance**

<sub>Comparing <code>dhruv/virtual-file</code> (482af9a) with <code>main</code> (21c5606)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-22 10:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-22 11:24_

---

_@MichaReiser approved on 2024-08-22 11:38_

Nice

---

_Merged by @dhruvmanila on 2024-08-23 07:04_

---

_Closed by @dhruvmanila on 2024-08-23 07:04_

---

_Branch deleted on 2024-08-23 07:04_

---
