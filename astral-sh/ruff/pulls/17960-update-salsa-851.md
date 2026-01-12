```yaml
number: 17960
title: "Update salsa (#851)"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/update-salsa-12
created_at: 2025-05-08T17:29:03Z
updated_at: 2025-05-09T09:39:21Z
url: https://github.com/astral-sh/ruff/pull/17960
synced_at: 2026-01-12T15:56:09Z
```

# Update salsa (#851)

---

_@MichaReiser_

_No description provided._

---

_Comment by @codspeed-hq[bot] on 2025-05-08 17:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fupdate-salsa-12)

### Merging #17960 will **improve performances by 8.6%**

<sub>Comparing <code>micha/update-salsa-12</code> (1fa5e1f) with <code>main</code> (981bd70)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_check_file[incremental] `` | 5.8 ms | 5.3 ms | +8.6% |


---

_Label `ty` added by @AlexWaygood on 2025-05-08 19:53_

---

_Comment by @github-actions[bot] on 2025-05-08 22:29_

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

_Closed by @MichaReiser on 2025-05-09 09:39_

---
