```yaml
number: 17847
title: Update salsa to pull in fixpoint fixes
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/perf-fixpoint-fix
created_at: 2025-05-05T06:42:21Z
updated_at: 2025-05-06T09:27:53Z
url: https://github.com/astral-sh/ruff/pull/17847
synced_at: 2026-01-12T15:56:06Z
```

# Update salsa to pull in fixpoint fixes

---

_@MichaReiser_

Pull in https://github.com/salsa-rs/salsa/pull/836 to get some fixpoint fixes.

---

_Comment by @github-actions[bot] on 2025-05-05 06:51_

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

_Label `ty` added by @MichaReiser on 2025-05-05 07:21_

---

_Closed by @MichaReiser on 2025-05-06 06:53_

---

_Reopened by @MichaReiser on 2025-05-06 07:34_

---

_Renamed from "Perf fixpoint bugfix" to "Update salsa to pull in fixpoint fixes" by @MichaReiser on 2025-05-06 08:53_

---

_Marked ready for review by @MichaReiser on 2025-05-06 08:53_

---

_Merged by @MichaReiser on 2025-05-06 09:27_

---

_Closed by @MichaReiser on 2025-05-06 09:27_

---

_Branch deleted on 2025-05-06 09:27_

---
