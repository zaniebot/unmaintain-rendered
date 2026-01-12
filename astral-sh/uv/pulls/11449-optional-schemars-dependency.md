```yaml
number: 11449
title: Optional schemars dependency
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/optional-jsonschema
created_at: 2025-02-12T16:06:10Z
updated_at: 2025-02-12T17:20:21Z
url: https://github.com/astral-sh/uv/pull/11449
synced_at: 2026-01-12T16:09:51Z
```

# Optional schemars dependency

---

_@konstin_

In preparation for `uv-build`, make schemars an optional dependency consistently, so it will not be part of the `uv-build` dependency tree.

---

_Label `internal` added by @konstin on 2025-02-12 16:06_

---

_@charliermarsh approved on 2025-02-12 16:12_

---

_@charliermarsh reviewed on 2025-02-12 16:13_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/Cargo.toml`:22 on 2025-02-12 16:13_

Does this need to be enabled by a schemars feature?

---

_@konstin reviewed on 2025-02-12 17:10_

---

_Review comment by @konstin on `crates/uv-pypi-types/Cargo.toml`:22 on 2025-02-12 17:10_

good catch

---

_Merged by @konstin on 2025-02-12 17:20_

---

_Closed by @konstin on 2025-02-12 17:20_

---

_Branch deleted on 2025-02-12 17:20_

---
