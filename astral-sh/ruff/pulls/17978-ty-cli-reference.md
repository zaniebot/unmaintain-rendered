```yaml
number: 17978
title: "[ty] CLI reference"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/ty-cli-reference
created_at: 2025-05-09T11:58:31Z
updated_at: 2025-05-09T12:26:25Z
url: https://github.com/astral-sh/ruff/pull/17978
synced_at: 2026-01-10T18:57:03Z
```

# [ty] CLI reference

---

_Pull request opened by @MichaReiser on 2025-05-09 11:58_

_No description provided._

---

_Label `documentation` added by @MichaReiser on 2025-05-09 11:58_

---

_Label `ty` added by @MichaReiser on 2025-05-09 11:58_

---

_@MichaReiser reviewed on 2025-05-09 12:00_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:1 on 2025-05-09 12:00_

I needed access to `Args` (now `Cli`) which is why I had to introduce a `lib.rs`. 

To avoid repeating the `mod` in both `lib` and `main.rs`, I decided to move the `run` logic to `lib.rs`

---

_@MichaReiser reviewed on 2025-05-09 12:00_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_cli_reference.rs`:1 on 2025-05-09 12:00_

This is copy paste from uv 

---

_Comment by @github-actions[bot] on 2025-05-09 12:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-05-09 12:04_

---

_Review requested from @carljm by @MichaReiser on 2025-05-09 12:04_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-09 12:04_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-09 12:04_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-09 12:04_

---

_Comment by @MichaReiser on 2025-05-09 12:04_

I'll merge this without review. This is all code that's easy to replace if anyone dares to touch it 😅 

---

_@MichaReiser reviewed on 2025-05-09 12:05_

---

_Review comment by @MichaReiser on `crates/ruff_dev/Cargo.toml`:36 on 2025-05-09 12:05_

this is a bit unfortunate but implementing a markdown to HTML converter is a bit out of scope right now ;)

---

_Comment by @github-actions[bot] on 2025-05-09 12:13_

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

_Merged by @MichaReiser on 2025-05-09 12:23_

---

_Closed by @MichaReiser on 2025-05-09 12:23_

---

_Branch deleted on 2025-05-09 12:23_

---
