```yaml
number: 9362
title: "Incorrect `Never` fix from `ANN201`"
type: issue
state: closed
author: tylerlaprade
labels: []
assignees: []
created_at: 2024-01-02T18:07:23Z
updated_at: 2024-01-02T18:07:58Z
url: https://github.com/astral-sh/ruff/issues/9362
synced_at: 2026-01-10T11:09:51Z
```

# Incorrect `Never` fix from `ANN201`

---

_Issue opened by @tylerlaprade on 2024-01-02 18:07_

I return values only inside `if` blocks, and if none of the `if` conditions trigger, I throw an exception. The autofix added `-> Never`, presumably because it was only looking outside the `if` blocks`.
<img width="827" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/13e813a2-0a2f-4015-b85d-1a17152a24c4">


---

_Comment by @charliermarsh on 2024-01-02 18:07_

I believe this is fixed on `main` and in the release that's going out today. If not, I will reopen!

---

_Closed by @charliermarsh on 2024-01-02 18:07_

---
