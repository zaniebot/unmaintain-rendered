```yaml
number: 17822
title: "Use `#[expect(lint)]` over `#[allow(lint)]` where possible"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/use-expect
created_at: 2025-05-03T18:40:24Z
updated_at: 2025-05-03T19:20:33Z
url: https://github.com/astral-sh/ruff/pull/17822
synced_at: 2026-01-10T18:57:03Z
```

# Use `#[expect(lint)]` over `#[allow(lint)]` where possible

---

_Pull request opened by @MichaReiser on 2025-05-03 18:40_

## Summary

`#[expect(lint)]` has the advantage that clippy warns if the code no longer raises said lint rule. 

This allowed me to remove some no longer needed `#[allow]` suppressions.



---

_Review requested from @carljm by @MichaReiser on 2025-05-03 18:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-03 18:40_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-03 18:40_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-03 18:40_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-05-03 18:40_

---

_Label `internal` added by @MichaReiser on 2025-05-03 18:40_

---

_Renamed from "Use `#[expect(lint)]` over `#[allow(lint)] where possible" to "Use `#[expect(lint)]` over `#[allow(lint)]` where possible" by @MichaReiser on 2025-05-03 18:40_

---

_Comment by @github-actions[bot] on 2025-05-03 18:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Closed by @MichaReiser on 2025-05-03 18:50_

---

_Reopened by @MichaReiser on 2025-05-03 18:51_

---

_@AlexWaygood approved on 2025-05-03 19:12_

---

_Comment by @github-actions[bot] on 2025-05-03 19:15_

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

_Merged by @MichaReiser on 2025-05-03 19:20_

---

_Closed by @MichaReiser on 2025-05-03 19:20_

---

_Branch deleted on 2025-05-03 19:20_

---
