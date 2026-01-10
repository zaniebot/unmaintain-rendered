```yaml
number: 426
title: Simpler and more resilient pip compile tests
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: debug-ci
created_at: 2023-11-15T08:30:39Z
updated_at: 2023-11-15T17:32:34Z
url: https://github.com/astral-sh/uv/pull/426
synced_at: 2026-01-10T15:50:28Z
```

# Simpler and more resilient pip compile tests

---

_Pull request opened by @konstin on 2023-11-15 08:30_

The pip compile test now explicitly set their python version and `puffin venv` resolves e.g. `python3.12` correctly now. The venv creation is moved to a shared method

---

_Renamed from "Debug CI compile_numpy_py38" to "Simpler and more resilient pip compile tests" by @konstin on 2023-11-15 08:37_

---

_Comment by @konstin on 2023-11-15 08:55_

@charliermarsh Is there a reason why we create the venvs in a subprocess instead of through a function call?

---

_@zanieb approved on 2023-11-15 15:33_

---

_Comment by @charliermarsh on 2023-11-15 15:35_

@konstin — I think it makes sense for the integration tests to stick to the public crate interface to faithfully test the full workflow.

---

_Merged by @konstin on 2023-11-15 17:32_

---

_Closed by @konstin on 2023-11-15 17:32_

---

_Branch deleted on 2023-11-15 17:32_

---
