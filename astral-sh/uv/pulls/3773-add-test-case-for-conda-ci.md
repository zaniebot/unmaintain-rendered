```yaml
number: 3773
title: Add test case for Conda CI
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/debug-conda
created_at: 2024-05-22T20:51:29Z
updated_at: 2024-05-22T21:37:27Z
url: https://github.com/astral-sh/uv/pull/3773
synced_at: 2026-01-12T16:05:50Z
```

# Add test case for Conda CI

---

_@zanieb_

Integration test coverage for #3769 

Failure against `main` on https://github.com/astral-sh/uv/actions/runs/9198224186/job/25300510016?pr=3773

Rebased against #3771 to check for fix

---

_Comment by @notatallshaw-gts on 2024-05-22 21:01_

FYI, personally I would explicitly use Miniforge (https://github.com/conda-incubator/setup-miniconda?tab=readme-ov-file#example-10-miniforge) as it is open source (https://github.com/conda-forge/miniforge/blob/main/LICENSE) where as  Anaconda/Miniconda comes up with a ToS (https://legal.anaconda.com/policies/en/#terms-of-service).



---

_Label `testing` added by @zanieb on 2024-05-22 21:28_

---

_Merged by @zanieb on 2024-05-22 21:37_

---

_Closed by @zanieb on 2024-05-22 21:37_

---

_Branch deleted on 2024-05-22 21:37_

---
