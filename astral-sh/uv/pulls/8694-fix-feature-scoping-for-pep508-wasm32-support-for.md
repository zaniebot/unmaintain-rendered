```yaml
number: 8694
title: Fix feature scoping for pep508 wasm32 support for ruff
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/version-range4
created_at: 2024-10-30T10:00:40Z
updated_at: 2024-10-30T12:21:24Z
url: https://github.com/astral-sh/uv/pull/8694
synced_at: 2026-01-12T16:08:27Z
```

# Fix feature scoping for pep508 wasm32 support for ruff

---

_@konstin_

Use the `non-pep508-extensions` feature so that ruff can compile to the wasm32-unknown-unknown target, when only using the default features without `non-pep508-extensions`. Since uv always uses the `non-pep508-extensions` feature, it is a cosmetic change for uv.

---

_Label `internal` added by @konstin on 2024-10-30 10:00_

---

_Merged by @konstin on 2024-10-30 12:21_

---

_Closed by @konstin on 2024-10-30 12:21_

---

_Branch deleted on 2024-10-30 12:21_

---
