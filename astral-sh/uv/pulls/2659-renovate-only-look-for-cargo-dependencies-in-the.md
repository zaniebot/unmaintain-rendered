```yaml
number: 2659
title: "Renovate: only look for cargo dependencies in the `crates/` directory"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate-cargo-deps
created_at: 2024-03-25T19:38:55Z
updated_at: 2024-03-25T19:44:13Z
url: https://github.com/astral-sh/uv/pull/2659
synced_at: 2026-01-10T14:49:08Z
```

# Renovate: only look for cargo dependencies in the `crates/` directory

---

_Pull request opened by @AlexWaygood on 2024-03-25 19:38_

As discussed on Discord, we probably don't want Renovate trying to bump "dependency pins" such as https://github.com/astral-sh/uv/blob/f97f47a67b9113d2ca52813ee9b7778a126d6ef3/scripts/packages/maturin_editable/Cargo.toml#L1-L4

---

_@zanieb approved on 2024-03-25 19:42_

---

_Label `internal` added by @zanieb on 2024-03-25 19:42_

---

_Merged by @AlexWaygood on 2024-03-25 19:44_

---

_Closed by @AlexWaygood on 2024-03-25 19:44_

---

_Branch deleted on 2024-03-25 19:44_

---
