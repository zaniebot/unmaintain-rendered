```yaml
number: 15363
title: "[red-knot] Print non-string panic payloads and (sometimes) backtraces"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/panic-payload
created_at: 2025-01-08T22:59:17Z
updated_at: 2025-01-09T09:24:50Z
url: https://github.com/astral-sh/ruff/pull/15363
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Print non-string panic payloads and (sometimes) backtraces

---

_Pull request opened by @dcreager on 2025-01-08 22:59_

More refinements to the panic messages for failing mdtests to mimic the output of the default panic hook more closely:

- We now print out `Box<dyn Any>` if the panic payload is not a string (which is typically the case for salsa panics).
- We now include the panic's backtrace if you set the `RUST_BACKTRACE` environment variable.

---

_Review requested from @carljm by @dcreager on 2025-01-08 22:59_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-08 22:59_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-08 22:59_

---

_Review requested from @sharkdp by @dcreager on 2025-01-08 22:59_

---

_Review comment by @dcreager on `crates/ruff_db/src/panic.rs`:8 on 2025-01-08 23:00_

Note that we can't put the actual payload here, because what we get back is a _reference_ to a `dyn Any`, and that does not guarantee that the payload is cloneable.

---

_Review comment by @dcreager on `crates/red_knot_test/src/lib.rs`:153 on 2025-01-08 23:01_

So instead we just mimic the output of the default panic hook here if the payload is not a string.  (Note that neither us nor `std::panic` can print out anything more meaningful, since you can only downcast `Any` to specific known types, not to traits like `Display` or `Debug`)

---

_@carljm approved on 2025-01-08 23:01_

Great, thank you!

---

_@dcreager reviewed on 2025-01-08 23:02_

---

_Comment by @github-actions[bot] on 2025-01-08 23:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dcreager on 2025-01-08 23:12_

---

_Closed by @dcreager on 2025-01-08 23:12_

---

_Branch deleted on 2025-01-08 23:12_

---

_Label `red-knot` added by @MichaReiser on 2025-01-09 09:24_

---
