```yaml
number: 17879
title: "[ty] Support `generate-shell-completion`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-shell-completion
created_at: 2025-05-06T05:27:15Z
updated_at: 2025-05-07T01:27:38Z
url: https://github.com/astral-sh/ruff/pull/17879
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Support `generate-shell-completion`

---

_Pull request opened by @InSyncWithFoo on 2025-05-06 05:27_

## Summary

Resolves #15502.

`ty generate-shell-completion` now works in a similar manner to `ruff generate-shell-completion`.

## Test Plan

Manually:

<details>

```shell
$ cargo run --package ty generate-shell-completion nushell
module completions {

  # An extremely fast Python type checker.
  export extern ty [
    --help(-h)                # Print help
    --version(-V)             # Print version
  ]
  
  # ...

}

export use completions *
```
</details>


---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-06 05:27_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-05-06 05:27_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-06 05:27_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-06 05:27_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-06 05:27_

---

_Comment by @github-actions[bot] on 2025-05-06 05:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-06 05:37_

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

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-06 06:37_

---

_Label `ty` added by @AlexWaygood on 2025-05-06 06:42_

---

_@MichaReiser approved on 2025-05-06 15:38_

Thanks

I've come around to accept `ty generate-shell-completion` because I think `ty completion` might be confusing, considering that ty provides completions in editors.

---

_Comment by @MichaReiser on 2025-05-06 15:38_

This is ready to go but requires a rebase

---

_Review request for @carljm removed by @carljm on 2025-05-06 17:37_

---

_Merged by @carljm on 2025-05-07 01:04_

---

_Closed by @carljm on 2025-05-07 01:04_

---

_Branch deleted on 2025-05-07 01:27_

---
