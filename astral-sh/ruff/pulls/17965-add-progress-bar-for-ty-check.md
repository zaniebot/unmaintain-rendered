```yaml
number: 17965
title: "Add progress bar for `ty check`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: ibraheem/progress-bar
created_at: 2025-05-08T20:09:22Z
updated_at: 2025-05-09T17:32:29Z
url: https://github.com/astral-sh/ruff/pull/17965
synced_at: 2026-01-12T15:56:09Z
```

# Add progress bar for `ty check`

---

_@ibraheemdev_

## Summary

Adds a simple progress bar for the `ty check` CLI command. The style is taken from uv, and like uv the bar is always shown - for smaller projects it is fast enough that it isn't noticeable. We could alternatively hide it completely based on some heuristic for the number of files, or only show it after some amount of time.

I also disabled it when `--watch` is passed, cancelling inflight checks was leading to zombie progress bars. I think we can fix this by using [`MultiProgress`](https://docs.rs/indicatif/latest/indicatif/struct.MultiProgress.html) and managing all the bars globally, but I left that out for now.

Resolves https://github.com/astral-sh/ty/issues/98.

## Test Plan

https://github.com/user-attachments/assets/1e6b76c6-f6cb-4967-8194-e774879b6dd1

---

_Review requested from @carljm by @ibraheemdev on 2025-05-08 20:09_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-05-08 20:09_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-05-08 20:09_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-05-08 20:09_

---

_Review requested from @dcreager by @ibraheemdev on 2025-05-08 20:09_

---

_Label `cli` added by @ibraheemdev on 2025-05-08 20:09_

---

_Label `ty` added by @ibraheemdev on 2025-05-08 20:09_

---

_Comment by @github-actions[bot] on 2025-05-08 20:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-08 20:25_

This looks great.

I'm a bit concerned that this logic is now inside the ty_projects crate.  The crate tries (it doesn't fully succeed) to be mostly CLI independent because it's used from wasm, the lsp and the CLI

Would it be much harder if the progress reporting would be injected from outside so that each implementation can decide on how to report progress?

---

_Comment by @ibraheemdev on 2025-05-08 20:28_

> I'm a bit concerned that this logic is now inside the ty_projects crate. The crate tries (it doesn't fully succeed) to be mostly CLI independent because it's used from wasm, the lsp and the CLI

Ah I was wondering about that but wasn't fully sure. I can move the logic out.

---

_Comment by @github-actions[bot] on 2025-05-08 20:30_

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

_Review request for @carljm removed by @carljm on 2025-05-08 23:10_

---

_Comment by @carljm on 2025-05-08 23:11_

This is awesome! I'll prefer to let Micha review the code.

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:236 on 2025-05-09 06:27_

Nit: Should we have a `run` and a `run_with_progress` method. It would reduce the amount of changes necessary (and you could implement `Reporter` for `()`)

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:217 on 2025-05-09 06:28_

Huh. I didn't know this works, but makes sense! 

---

_@MichaReiser approved on 2025-05-09 06:29_

---

_Review comment by @ibraheemdev on `crates/ty/src/main.rs`:236 on 2025-05-09 15:11_

I try to avoid implementing traits for `()`, I find a named unit struct clearer, but I don't feel to strongly.

---

_@ibraheemdev reviewed on 2025-05-09 15:11_

---

_@MichaReiser reviewed on 2025-05-09 15:17_

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:236 on 2025-05-09 15:17_

I also have no strong preferences. 

---

_Merged by @ibraheemdev on 2025-05-09 17:32_

---

_Closed by @ibraheemdev on 2025-05-09 17:32_

---

_Branch deleted on 2025-05-09 17:32_

---
