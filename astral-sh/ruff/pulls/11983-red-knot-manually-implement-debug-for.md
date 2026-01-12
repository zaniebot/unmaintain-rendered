```yaml
number: 11983
title: "[red-knot] Manually implement `Debug` for `VendoredFileSystem`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: vendored-filesystem-convenience
created_at: 2024-06-22T14:13:26Z
updated_at: 2024-06-23T13:25:57Z
url: https://github.com/astral-sh/ruff/pull/11983
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Manually implement `Debug` for `VendoredFileSystem`

---

_@AlexWaygood_

I temporarily added this in a PR to my fork so I could debug some test failures on Windows that were a result of paths not being normalized when the zip archive was being created at build time. It seems like it could be a useful method to have around generally, in case we need to debug similar issues in the future.

---

_Label `red-knot` added by @AlexWaygood on 2024-06-22 14:13_

---

_Review requested from @carljm by @AlexWaygood on 2024-06-22 14:13_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-22 14:13_

---

_Comment by @github-actions[bot] on 2024-06-22 14:27_

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

_@MichaReiser reviewed on 2024-06-22 16:52_

I'm in favor of easing debugging but I'm worried that the way it is done in this PR leaks abstraction and it has no safeguards (other than a comment) that prevents actual usage of the method (it's public, no conditional compilation). 

I recommend to instead create a manual `Debug` implementation. This preserves the encapsulation and gives a better debugging experience by default, without having to know about the "file_names" function (just use `dbg!`). 

The way I would implement `Debug` is by testing for the [`alternate`](https://doc.rust-lang.org/std/fmt/struct.Formatter.html#method.alternate) flag and if it is set, emit a map with all the file names (and maybe the size of each file?) using either `f.debug_map` or `f.debug_list`. If `alternate` isn't set, maybe just print the number of files?


---

_Comment by @AlexWaygood on 2024-06-22 16:54_

That's a much better idea -- thanks!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-22 18:19_

---

_Renamed from "[red-knot] Add a convenience method to `VendoredFileSystem` to ease debugging" to "[red-knot] Manually implement `Debug` and `Display` for `VendoredFileSystem`" by @AlexWaygood on 2024-06-22 18:20_

---

_@MichaReiser reviewed on 2024-06-22 18:38_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:148 on 2024-06-22 18:38_

It's unclear to me what the benefit of implementing `Display` is. `Display` is mainly used as a representation shown to the user and it's unclear to me where this is needed. And if we need it, I think `debug_struct` isn't the right representation for it.

---

_@MichaReiser reviewed on 2024-06-22 18:40_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/vendored.rs`:336 on 2024-06-22 18:40_

I'm slightly confused. Why does the manual debug still render the inner `Mutex` considering that it is implemented on `VendoredFileSystem`.

Ah, it's because it's implemented on the `Archive`. I would just implement it on `VendoredFileSystem`. The archive representation isn't relevant for `VendoredFileSystem` users.

---

_Renamed from "[red-knot] Manually implement `Debug` and `Display` for `VendoredFileSystem`" to "[red-knot] Manually implement `Debug` for `VendoredFileSystem`" by @AlexWaygood on 2024-06-23 12:01_

---

_@AlexWaygood reviewed on 2024-06-23 12:02_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:148 on 2024-06-23 12:02_

Fair enough, I removed `Display`!

---

_@AlexWaygood reviewed on 2024-06-23 12:05_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/vendored.rs`:336 on 2024-06-23 12:05_

Agreed, though for debugging purposes I think it would be useful to know if the inner mutex were unexpectedly poisoned for some reason. I've just reworked it a bit to expose more information about the inner zip files (when the alternate debug representation has been specified) but expose less about the intermediate types between the zip archive and the `VendoredFileSystem` struct... let me know what you think!

---

_Comment by @codspeed-hq[bot] on 2024-06-23 12:06_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/vendored-filesystem-convenience)

### Merging #11983 will **degrade performances by 4.82%**

<sub>Comparing <code>vendored-filesystem-convenience</code> (5a053ef) with <code>main</code> (7156096)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/vendored-filesystem-convenience)._

### Benchmarks breakdown

|     | Benchmark | `main` | `vendored-filesystem-convenience` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.82% |


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-23 12:23_

---

_@MichaReiser approved on 2024-06-23 13:18_

This is fancy!

---

_Merged by @AlexWaygood on 2024-06-23 13:25_

---

_Closed by @AlexWaygood on 2024-06-23 13:25_

---

_Branch deleted on 2024-06-23 13:25_

---
