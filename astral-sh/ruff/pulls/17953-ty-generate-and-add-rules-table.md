```yaml
number: 17953
title: "[ty] Generate and add rules table"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/ty-rules-table
created_at: 2025-05-08T14:19:42Z
updated_at: 2025-05-08T15:00:36Z
url: https://github.com/astral-sh/ruff/pull/17953
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Generate and add rules table

---

_@MichaReiser_

## Summary

This PR adds a script to generate a markdown file documenting all known ty rules. 

I plan to link to this rule table from ty's README.

## Test Plan

See commited `rules.md`

https://github.com/astral-sh/ruff/blob/micha/ty-rules-table/crates/ty/docs/rules.md


---

_Label `documentation` added by @MichaReiser on 2025-05-08 14:19_

---

_Label `ty` added by @MichaReiser on 2025-05-08 14:19_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_rules.rs`:102 on 2025-05-08 14:20_

url encoding is surprisingly hard lol

---

_@MichaReiser reviewed on 2025-05-08 14:20_

---

_Comment by @github-actions[bot] on 2025-05-08 14:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-08 14:29_

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

_Marked ready for review by @MichaReiser on 2025-05-08 14:39_

---

_Review requested from @carljm by @MichaReiser on 2025-05-08 14:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-08 14:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-08 14:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-08 14:39_

---

_@carljm approved on 2025-05-08 14:43_

Looks good to me! Thank you.

---

_Review comment by @carljm on `crates/ruff_dev/src/generate_ty_rules.rs`:102 on 2025-05-08 14:48_

it's like CI was reading this comment

---

_@carljm reviewed on 2025-05-08 14:48_

---

_Merged by @MichaReiser on 2025-05-08 14:55_

---

_Closed by @MichaReiser on 2025-05-08 14:55_

---

_Branch deleted on 2025-05-08 14:55_

---
