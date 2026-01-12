```yaml
number: 8365
title: Ensure that ecosystem check job fails if the tooling encounters an unexpected error
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/ecosystem-fail
created_at: 2023-10-30T20:24:05Z
updated_at: 2023-10-31T00:48:39Z
url: https://github.com/astral-sh/ruff/pull/8365
synced_at: 2026-01-12T15:55:26Z
```

# Ensure that ecosystem check job fails if the tooling encounters an unexpected error

---

_@zanieb_

Previously, `| tee` would hide bad exit codes from `ruff-ecosystem ...`

See poc failure at https://github.com/astral-sh/ruff/actions/runs/6698487019/job/18200852648?pr=8365

---

_Label `internal` added by @zanieb on 2023-10-30 20:24_

---

_Comment by @github-actions[bot] on 2023-10-30 20:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_@charliermarsh approved on 2023-10-30 23:28_

---

_Merged by @zanieb on 2023-10-31 00:48_

---

_Closed by @zanieb on 2023-10-31 00:48_

---

_Branch deleted on 2023-10-31 00:48_

---
