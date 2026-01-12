```yaml
number: 11922
title: "Edition 2024 prep: Escape `r#gen` and remove redundant ref"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/edition-2025-min
created_at: 2025-03-03T11:05:23Z
updated_at: 2025-03-03T11:13:57Z
url: https://github.com/astral-sh/uv/pull/11922
synced_at: 2026-01-12T16:10:04Z
```

# Edition 2024 prep: Escape `r#gen` and remove redundant ref

---

_@konstin_

Three edition 2021 compatible sets of changes in preparation for the edition 2025 split out from #11724.

In edition 2025, `gen` is a keyword, so we escape it as `r#gen`. `ref` and `ref mut` are not allowed anymore for `&T` and `&mut T`, so we remove them. `cargo fmt` now formats inside of macros, which the 2021 formatter doesn't undo.

---

_Label `internal` added by @konstin on 2025-03-03 11:05_

---

_Merged by @konstin on 2025-03-03 11:13_

---

_Closed by @konstin on 2025-03-03 11:13_

---

_Branch deleted on 2025-03-03 11:13_

---
