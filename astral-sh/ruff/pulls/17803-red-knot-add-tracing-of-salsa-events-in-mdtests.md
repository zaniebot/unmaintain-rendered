```yaml
number: 17803
title: "[red-knot] add tracing of salsa events in mdtests"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/mdtest-tracing
created_at: 2025-05-03T00:27:30Z
updated_at: 2025-05-03T07:00:13Z
url: https://github.com/astral-sh/ruff/pull/17803
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] add tracing of salsa events in mdtests

---

_Pull request opened by @carljm on 2025-05-03 00:27_

## Summary

This helps with debugging Salsa issues reproduced in an mdtest, since it makes Salsa events (such as the `WillIterateCycle` event) visible in tracing.

## Test Plan

Used `log = "red_knot_test=trace"` in an mdtest and saw salsa events appear in the tracing output from running that test.




---

_Label `red-knot` added by @carljm on 2025-05-03 00:27_

---

_Review requested from @MichaReiser by @carljm on 2025-05-03 00:27_

---

_Review requested from @AlexWaygood by @carljm on 2025-05-03 00:27_

---

_Review requested from @sharkdp by @carljm on 2025-05-03 00:27_

---

_Review requested from @dcreager by @carljm on 2025-05-03 00:27_

---

_Comment by @github-actions[bot] on 2025-05-03 00:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager approved on 2025-05-03 00:34_

---

_Comment by @github-actions[bot] on 2025-05-03 00:37_

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

_Merged by @MichaReiser on 2025-05-03 07:00_

---

_Closed by @MichaReiser on 2025-05-03 07:00_

---

_Branch deleted on 2025-05-03 07:00_

---
