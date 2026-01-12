```yaml
number: 1671
title: Improve Pandas call and attribute detection
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pd
created_at: 2023-01-06T00:30:36Z
updated_at: 2023-01-06T00:30:55Z
url: https://github.com/astral-sh/ruff/pull/1671
synced_at: 2026-01-12T05:36:32Z
```

# Improve Pandas call and attribute detection

---

_Pull request opened by @charliermarsh on 2023-01-06 00:30_

This PR adds some guardrails to avoid common false positives in our `pandas-vet` rules. Specifically, we now avoid triggering `pandas-vet` rules if the target of the call or attribute (i.e., the `x` in `x.stack(...)`) is unbound, or bound to something that couldn't be a DataFrame (like an import that _isn't_ `pandas`, or a class definition). This lets us avoid common false positives like `np.stack(...)`.

Resolves #1659.


---

_Merged by @charliermarsh on 2023-01-06 00:30_

---

_Closed by @charliermarsh on 2023-01-06 00:30_

---

_Branch deleted on 2023-01-06 00:30_

---
