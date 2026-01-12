```yaml
number: 17964
title: "[ty] Update salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-854
created_at: 2025-05-08T19:55:11Z
updated_at: 2025-05-09T09:56:13Z
url: https://github.com/astral-sh/ruff/pull/17964
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Update salsa

---

_@MichaReiser_

This PR updates salsa to get the new `returns(ref)`, `returns(deref)`, `returns(as_ref)` and `retruns(as_deref)` (in addition to the existing `returns(clone)`, `returns(ref)` and `returns_copy)`. 

This helps simplify some of our code.

This PR also pulls in a perf improvement related to incremental verification of queries involving fixpoint.

---

_Comment by @codspeed-hq[bot] on 2025-05-08 20:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-854)

### Merging #17964 will **improve performances by 8.32%**

<sub>Comparing <code>micha/salsa-854</code> (1c2c8c0) with <code>main</code> (12ce445)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_check_file[incremental] `` | 5.8 ms | 5.4 ms | +8.32% |


---

_Label `ty` added by @AlexWaygood on 2025-05-08 22:22_

---

_Comment by @github-actions[bot] on 2025-05-08 22:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Renamed from "[ty] Update salsa (#854)" to "[ty] Update salsa" by @MichaReiser on 2025-05-09 09:14_

---

_Label `internal` added by @MichaReiser on 2025-05-09 09:14_

---

_Marked ready for review by @MichaReiser on 2025-05-09 09:39_

---

_Review requested from @carljm by @MichaReiser on 2025-05-09 09:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-09 09:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-09 09:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-09 09:39_

---

_Comment by @github-actions[bot] on 2025-05-09 09:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-05-09 09:54_

---

_Closed by @MichaReiser on 2025-05-09 09:54_

---

_Branch deleted on 2025-05-09 09:54_

---
