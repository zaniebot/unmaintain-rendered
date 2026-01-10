```yaml
number: 14214
title: Consolidate logic for checking for a virtual environment
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/consolidate-virtualenv-check
created_at: 2025-06-23T11:51:41Z
updated_at: 2025-06-23T13:12:45Z
url: https://github.com/astral-sh/uv/pull/14214
synced_at: 2026-01-10T11:10:43Z
```

# Consolidate logic for checking for a virtual environment

---

_Pull request opened by @jtfmumm on 2025-06-23 11:51_

We were checking whether a path was an executable in a virtual environment or the base directory of a virtual environment in multiple places in the codebase. This PR consolidates this logic into one place.

Closes #13947.


---

_Label `internal` added by @jtfmumm on 2025-06-23 11:51_

---

_@konstin approved on 2025-06-23 13:02_

---

_Merged by @jtfmumm on 2025-06-23 13:12_

---

_Closed by @jtfmumm on 2025-06-23 13:12_

---

_Branch deleted on 2025-06-23 13:12_

---
