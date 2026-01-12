```yaml
number: 17937
title: "[ty] Update salsa"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/update-salsa
created_at: 2025-05-08T08:09:30Z
updated_at: 2025-05-08T10:02:55Z
url: https://github.com/astral-sh/ruff/pull/17937
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Update salsa

---

_@sharkdp_

## Summary

* Update salsa to pull in https://github.com/salsa-rs/salsa/pull/850.
* Some refactoring of salsa event callbacks in various `Db`'s due to https://github.com/salsa-rs/salsa/pull/849

closes https://github.com/astral-sh/ty/issues/108

## Test Plan

Ran `cargo run --bin ty -- -vvv` on a test file to make sure that salsa Events are still logged.

---

_Review requested from @carljm by @sharkdp on 2025-05-08 08:09_

---

_Label `ty` added by @sharkdp on 2025-05-08 08:09_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-08 08:09_

---

_Review requested from @dcreager by @sharkdp on 2025-05-08 08:09_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-08 08:09_

---

_Comment by @github-actions[bot] on 2025-05-08 08:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:52 on 2025-05-08 08:14_

`tracing::enabled` shouldn't change during a run. Therefore, it would be better to set the event handler to `None` if the `trace` level isn't enabled.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-08 08:14_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/db.rs`:71 on 2025-05-08 08:15_

This comment is no longer accurate. Can you say more why `get_mut` doesn't work here anymore?

---

_@MichaReiser reviewed on 2025-05-08 08:16_

Thank you

---

_@sharkdp reviewed on 2025-05-08 08:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/db.rs`:71 on 2025-05-08 08:21_

Right, I also just noticed that and it's already removed.

`Arc::get_mut` requires exclusive access (to the content, i.e. the `Mutex<…>`). But we cloned the `Arc` above. There is a reference to the content (`Mutex`) captured in the event handler callback. But we don't need exclusive access to the `Mutex`! We need exclusive access to the `Vec` stored *inside* the `Mutex`. But that's what `Mutex` is for. We just need to get a `.lock()`.

---

_@MichaReiser reviewed on 2025-05-08 08:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/db.rs`:71 on 2025-05-08 08:24_

Let me have a closer look. I do think that using an `Arc` here was intentional to enforce the constraint that it's never possible to take the events while holding a reference to another db (`db` is `Clone`)

---

_@MichaReiser approved on 2025-05-08 08:27_

This is great. Thanks for doing it. Looks good, but we should set the event handler to `None` if the `trace` level is disabled (which should always be true in release builds)

---

_Comment by @github-actions[bot] on 2025-05-08 08:27_

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

_@sharkdp reviewed on 2025-05-08 08:30_

---

_Review comment by @sharkdp on `crates/ty_project/src/db.rs`:52 on 2025-05-08 08:30_

Done. I tried a few variations because I wanted to pull it out into a variable (and potentially use `bool::then`), but that requires a complex type annotations, so I just left it inline.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/db.rs`:71 on 2025-05-08 08:36_

Oh, that might explain the use of `Arc::get_mut`. In this case, should we have an additional `count: Arc<()>` member (or similar) to enforce that constraint?

---

_@sharkdp reviewed on 2025-05-08 08:36_

---

_Closed by @sharkdp on 2025-05-08 08:36_

---

_Reopened by @sharkdp on 2025-05-08 08:36_

---

_Closed by @sharkdp on 2025-05-08 08:54_

---

_Reopened by @sharkdp on 2025-05-08 08:54_

---

_Merged by @sharkdp on 2025-05-08 10:02_

---

_Closed by @sharkdp on 2025-05-08 10:02_

---

_Branch deleted on 2025-05-08 10:02_

---
