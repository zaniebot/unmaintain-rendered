```yaml
number: 14356
title: "[red-knot] Mark LoggingGuard as `must_use`"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/logging-guard-must_use
created_at: 2024-11-15T10:49:23Z
updated_at: 2024-11-15T11:47:27Z
url: https://github.com/astral-sh/ruff/pull/14356
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Mark LoggingGuard as `must_use`

---

_Pull request opened by @sharkdp on 2024-11-15 10:49_

## Summary

Prevents developers like me from trying to use
```rs
ruff_db::testing::setup_logging();
```
without capturing the returned guard in a variable:
```
warning: unused `LoggingGuard` that must be used
  --> crates/red_knot_workspace/tests/check.rs:24:5
   |
24 |     ruff_db::testing::setup_logging();
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: Dropping the guard unregisters the tracing subscriber.
   = note: `#[warn(unused_must_use)]` on by default
```

I'm not sure why the inner `tracing::subscriber::DefaultGuard` is not marked as `must_use` in `tracing`, but they do mark the return value of `tracing::subscriber::set_default` as `must_use`:

https://github.com/tokio-rs/tracing/blob/15600a3a67c418f53cb80ff21da57d89a5de0486/tracing/src/subscriber.rs#L57

That doesn't trigger anymore because we store the result in `LoggingBuilder::build()` here:

https://github.com/astral-sh/ruff/blob/1f82731856eb0f199c8b11f8e06c32020a6a9738/crates/ruff_db/src/testing.rs#L182-L206

An alternative might be to mark both `setup_logging` and `setup_logging_with_filter` as `must_use`.

## Test Plan

Run `cargo test` with the code above and observe the compiler warning.

---

_Label `internal` added by @sharkdp on 2024-11-15 10:49_

---

_Label `red-knot` added by @sharkdp on 2024-11-15 10:49_

---

_Review requested from @carljm by @sharkdp on 2024-11-15 10:49_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-15 10:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-15 10:49_

---

_@AlexWaygood approved on 2024-11-15 10:52_

---

_Comment by @github-actions[bot] on 2024-11-15 11:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-11-15 11:47_

Ah nice!

---

_Merged by @MichaReiser on 2024-11-15 11:47_

---

_Closed by @MichaReiser on 2024-11-15 11:47_

---

_Branch deleted on 2024-11-15 11:47_

---
