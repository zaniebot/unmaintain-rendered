```yaml
number: 19773
title: "[ty] Avoid overcounting shared memory usage"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/get-size-tracker
created_at: 2025-08-05T21:34:38Z
updated_at: 2025-08-06T19:32:04Z
url: https://github.com/astral-sh/ruff/pull/19773
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Avoid overcounting shared memory usage

---

_@ibraheemdev_

## Summary

Use a global tracker to avoid double counting `Arc` instances.

---

_Review requested from @carljm by @ibraheemdev on 2025-08-05 21:34_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-08-05 21:34_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-08-05 21:34_

---

_Label `internal` added by @ibraheemdev on 2025-08-05 21:34_

---

_Review requested from @dcreager by @ibraheemdev on 2025-08-05 21:34_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-08-05 21:34_

---

_Label `ty` added by @ibraheemdev on 2025-08-05 21:34_

---

_Comment by @github-actions[bot] on 2025-08-05 21:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 21:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~66MB
+ TOTAL MEMORY USAGE: ~69MB
-     memo fields = ~52MB
+     memo fields = ~54MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~159MB
+ TOTAL MEMORY USAGE: ~167MB
-     memo fields = ~119MB
+     memo fields = ~125MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo fields = ~204MB
+     memo fields = ~214MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~424MB
+     memo fields = ~445MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-08-05 21:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_memory_usage/src/lib.rs`:14 on 2025-08-06 06:04_

What I understand is that all `get_size` functions are called twice now. Once to collect the struct and once to collect the query heap memory usage. Because of that, I think we'll now be underreporting memory usage when using a single tracker.

---

_@MichaReiser reviewed on 2025-08-06 06:04_

Would it be worth considering to vendor our own `get_size` (in ruff memory usage). It's not very complicated code. 

https://github.com/salsa-rs/salsa/pull/944 would allow us to avoid the global lock. 

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-06 07:17_

---

_Review comment by @ibraheemdev on `crates/ruff_memory_usage/src/lib.rs`:14 on 2025-08-06 16:55_

Should be fixed by https://github.com/salsa-rs/salsa/pull/964.

---

_@ibraheemdev reviewed on 2025-08-06 16:55_

---

_Review comment by @bircni on `Cargo.toml`:87 on 2025-08-06 17:06_

```suggestion
get-size2 = { version = "0.6.2", features = [
```

---

_@bircni reviewed on 2025-08-06 17:06_

---

_@MichaReiser approved on 2025-08-06 18:06_

---

_Comment by @ibraheemdev on 2025-08-06 19:31_

It looks like `untracked_arc_size` was incorrect before in that it didn't account for the stack size of `T`, so the reported memory usage correctly goes up with this change.

---

_Merged by @ibraheemdev on 2025-08-06 19:32_

---

_Closed by @ibraheemdev on 2025-08-06 19:32_

---

_Branch deleted on 2025-08-06 19:32_

---
