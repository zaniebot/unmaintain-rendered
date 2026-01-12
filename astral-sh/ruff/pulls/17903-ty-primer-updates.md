```yaml
number: 17903
title: "[ty] primer updates"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/mrocycle-primer
created_at: 2025-05-07T02:08:00Z
updated_at: 2025-05-09T03:43:32Z
url: https://github.com/astral-sh/ruff/pull/17903
synced_at: 2026-01-12T15:56:07Z
```

# [ty] primer updates

---

_@carljm_

## Summary

Update ecosystem project lists in light of https://github.com/astral-sh/ruff/pull/17758

## Test Plan

CI on this PR.


---

_Review requested from @AlexWaygood by @carljm on 2025-05-07 02:08_

---

_Review requested from @sharkdp by @carljm on 2025-05-07 02:08_

---

_Review requested from @dcreager by @carljm on 2025-05-07 02:08_

---

_Label `ty` added by @carljm on 2025-05-07 02:08_

---

_Comment by @github-actions[bot] on 2025-05-07 02:17_

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

_Renamed from "Cjm/mrocycle primer" to "[ty] primer updates" by @carljm on 2025-05-07 03:23_

---

_Closed by @carljm on 2025-05-07 03:24_

---

_Reopened by @carljm on 2025-05-07 03:24_

---

_Comment by @github-actions[bot] on 2025-05-07 04:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-05-07 06:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/primer/bad.txt`:10 on 2025-05-07 06:23_

😨 

---

_@MichaReiser reviewed on 2025-05-07 06:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/primer/bad.txt`:10 on 2025-05-07 06:24_

```suggestion
pandas-stubs  # salsa cycle assertion fails (https://github.com/salsa-rs/salsa/issues/831)
```

---

_@MichaReiser approved on 2025-05-07 06:24_

---

_@MichaReiser reviewed on 2025-05-07 06:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/primer/bad.txt`:7 on 2025-05-07 06:45_

Does this also hang with my most recent fixpoint PR?

---

_@AlexWaygood reviewed on 2025-05-07 10:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/primer/bad.txt`:10 on 2025-05-07 10:20_

Does the assertion failure consistently reproduce when checking `pandas-stubs`? I just saw it occur in a mypy_primer run on https://github.com/astral-sh/ruff/pull/17897 when checking `trio` (<https://github.com/astral-sh/ruff/actions/runs/14880713337/job/41787973995>) but it went away when I reran the job.

---

_@carljm reviewed on 2025-05-09 03:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/primer/bad.txt`:7 on 2025-05-09 03:08_

Yes :/ Not every time, but often enough (even locally) that I don't think we should add it to `good.txt` yet.

---

_@carljm reviewed on 2025-05-09 03:15_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/primer/bad.txt`:10 on 2025-05-09 03:15_

Not consistently -- it either seems to hang, or it hits that assertion and then finishes.

---

_Merged by @carljm on 2025-05-09 03:43_

---

_Closed by @carljm on 2025-05-09 03:43_

---

_Branch deleted on 2025-05-09 03:43_

---
