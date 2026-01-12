```yaml
number: 9055
title: Refactor shell quoting
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/shlex-refactor
created_at: 2024-11-12T12:54:44Z
updated_at: 2024-11-15T09:06:55Z
url: https://github.com/astral-sh/uv/pull/9055
synced_at: 2026-01-12T16:08:37Z
```

# Refactor shell quoting

---

_@konstin_

Move the shlex-like quoting utils in the uv-shell crate, so we only write `r#"'"'"'"#` once.

Split out from #8984

---

_Label `enhancement` added by @konstin on 2024-11-12 12:54_

---

_Review comment by @konstin on `Cargo.toml`:165 on 2024-11-12 12:54_

Rustrover complained this was missing

---

_@konstin reviewed on 2024-11-12 12:54_

---

_Review requested from @charliermarsh by @konstin on 2024-11-13 11:34_

---

_@charliermarsh reviewed on 2024-11-15 04:05_

---

_Review comment by @charliermarsh on `Cargo.toml`:165 on 2024-11-15 04:05_

Strange, ok.

---

_@charliermarsh approved on 2024-11-15 04:05_

---

_@charliermarsh reviewed on 2024-11-15 04:05_

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/Cargo.toml`:33 on 2024-11-15 04:05_

Nit: Sort please

---

_@konstin reviewed on 2024-11-15 08:54_

---

_Review comment by @konstin on `crates/uv-install-wheel/Cargo.toml`:33 on 2024-11-15 08:54_

thx

---

_Merged by @konstin on 2024-11-15 09:06_

---

_Closed by @konstin on 2024-11-15 09:06_

---

_Branch deleted on 2024-11-15 09:06_

---
