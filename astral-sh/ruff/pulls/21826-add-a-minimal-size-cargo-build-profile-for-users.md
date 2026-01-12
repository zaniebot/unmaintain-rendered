```yaml
number: 21826
title: "Add a `minimal-size` cargo build profile for users who want to build a smaller binary"
type: pull_request
state: merged
author: lmmx
labels:
  - release
  - ty
assignees: []
merged: true
base: main
head: minimal-size-profile
created_at: 2025-12-06T17:33:08Z
updated_at: 2025-12-06T19:57:44Z
url: https://github.com/astral-sh/ruff/pull/21826
synced_at: 2026-01-12T15:57:34Z
```

# Add a `minimal-size` cargo build profile for users who want to build a smaller binary

---

_@lmmx_

This PR adds the same `minimal-size` profile as `uv` repo workspace has

```toml
# Profile to build a minimally sized binary for uv-build
[profile.minimal-size]
inherits = "release"
opt-level = "z"
# This will still show a panic message, we only skip the unwind
panic = "abort"
codegen-units = 1
```
but removes its `panic = "abort"` setting

- As discussed in #21825

Compared to the ones pre-built via `uv tool install`, this builds 35% smaller ruff and 24% smaller ty binaries
(as measured [here](https://github.com/lmmx/just-pre-commit/blob/master/refresh_binaries.sh))

---

_Renamed from "perf: add minimal-size profile (same as in uv)" to "perf: add minimal-size profile" by @lmmx on 2025-12-06 17:33_

---

_@Gankra approved on 2025-12-06 17:45_

No harm in including this, and convenient to be able to point people to if they complain about size.

---

_Comment by @astral-sh-bot[bot] on 2025-12-06 18:04_


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

_Merged by @Gankra on 2025-12-06 18:19_

---

_Closed by @Gankra on 2025-12-06 18:19_

---

_Renamed from "perf: add minimal-size profile" to "Add a `minimal-size` cargo build profile for users who want to build a smaller binary" by @Gankra on 2025-12-06 18:19_

---

_Label `ty` added by @Gankra on 2025-12-06 18:20_

---

_Label `release` added by @Gankra on 2025-12-06 18:20_

---

_Branch deleted on 2025-12-06 19:57_

---
