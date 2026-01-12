```yaml
number: 3484
title: "PYI011: allow `math` constants in defaults"
type: pull_request
state: merged
author: XuehaiPan
labels:
  - bug
assignees: []
merged: true
base: main
head: PYI011-math-consts
created_at: 2023-03-13T16:55:48Z
updated_at: 2023-03-13T18:24:34Z
url: https://github.com/astral-sh/ruff/pull/3484
synced_at: 2026-01-12T15:55:13Z
```

# PYI011: allow `math` constants in defaults

---

_@XuehaiPan_

Allow `math` constants in defaults:

- `math.inf`, `-math.inf`
- `math.nan` (reject `-math.nan`)
- `math.e`, `-math.e`
- `math.pi`, `-math.pi`
- `math.tau`, `-math.tau`

Refs:

- PyCQA/flake8-pyi#354
- PyCQA/flake8-pyi#355

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:39 on 2023-03-13 17:10_

Nit: let's rename this to `ALLOWED_SYS_ATTRIBUTES_IN_DEFAULTS`?

---

_@charliermarsh reviewed on 2023-03-13 17:10_

---

_Comment by @github-actions[bot] on 2023-03-13 17:15_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@XuehaiPan reviewed on 2023-03-13 17:17_

---

_Review comment by @XuehaiPan on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:39 on 2023-03-13 17:17_

This follows the variable name in flake8-pyi. I think nothing needs to change here. This list may extend in the future (e.g., `os.pathsep`). A separate variable for `math.*` due to they support `-` op (`-math.inf`).

---

_@charliermarsh reviewed on 2023-03-13 18:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:39 on 2023-03-13 18:22_

Ah ok, that's reasonable.

---

_Label `bug` added by @charliermarsh on 2023-03-13 18:22_

---

_Merged by @charliermarsh on 2023-03-13 18:23_

---

_Closed by @charliermarsh on 2023-03-13 18:23_

---

_Branch deleted on 2023-03-13 18:24_

---
