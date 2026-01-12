```yaml
number: 2369
title: Disable incompatible rules rather than merely warning
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/D-fix
created_at: 2023-01-30T23:39:25Z
updated_at: 2023-01-30T23:47:06Z
url: https://github.com/astral-sh/ruff/pull/2369
synced_at: 2026-01-12T15:55:08Z
```

# Disable incompatible rules rather than merely warning

---

_@charliermarsh_

This is another temporary fix for the problem described in #2289 and #2292. Rather than merely warning, we now disable the incompatible rules (in addition to the warning). I actually think this is quite a reasonable solution, but we can revisit later. I just can't bring myself to ship another release with autofix broken-by-default :joy:


---

_@charliermarsh reviewed on 2023-01-30 23:39_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:370 on 2023-01-30 23:39_

Previously, we were only warning for the _first_ incompatible rule pair, so users never saw the `D212` / `D213` warning!

---

_Merged by @charliermarsh on 2023-01-30 23:47_

---

_Closed by @charliermarsh on 2023-01-30 23:47_

---

_Branch deleted on 2023-01-30 23:47_

---
