```yaml
number: 13051
title: "Block scripts from overwriting `python`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/reserved-script-names
created_at: 2025-04-22T11:02:59Z
updated_at: 2025-04-25T07:10:11Z
url: https://github.com/astral-sh/uv/pull/13051
synced_at: 2026-01-12T16:10:32Z
```

# Block scripts from overwriting `python`

---

_@konstin_

uv adds some binaries and scripts to a venv, and installed packages should not be allowed to overwrite them.

Fixes #12983

---

_Label `bug` added by @konstin on 2025-04-22 11:03_

---

_Review requested from @zanieb by @konstin on 2025-04-22 11:03_

---

_Comment by @imann3198 on 2025-04-22 22:41_

[![CI](https://github.com/astral-sh/uv/actions/workflows/ci.yml/badge.svg)](https://github.com/astral-sh/uv/actions/workflows/ci.yml)

---

_@jtfmumm approved on 2025-04-23 15:44_

---

_@jtfmumm reviewed on 2025-04-23 15:45_

---

_Review comment by @jtfmumm on `crates/uv-install-wheel/src/wheel.rs`:190 on 2025-04-23 15:45_

Why not just check if `entrypoint.name` starts with "activate"?

---

_@konstin reviewed on 2025-04-23 16:04_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/wheel.rs`:190 on 2025-04-23 16:04_

It avoids detecting other scripts that are called something with `activate`, the warnings are intentionally scoped tightly to avoid false positives.

---

_Review comment by @charliermarsh on `crates/uv-install-wheel/src/wheel.rs`:197 on 2025-04-25 01:33_

Why a warning here rather than an error? (Just wondering.)

---

_@charliermarsh reviewed on 2025-04-25 01:33_

---

_@konstin reviewed on 2025-04-25 07:04_

---

_Review comment by @konstin on `crates/uv-install-wheel/src/wheel.rs`:197 on 2025-04-25 07:04_

We could also error here, but they didn't feel critical enough so far

---

_Merged by @konstin on 2025-04-25 07:10_

---

_Closed by @konstin on 2025-04-25 07:10_

---

_Branch deleted on 2025-04-25 07:10_

---
