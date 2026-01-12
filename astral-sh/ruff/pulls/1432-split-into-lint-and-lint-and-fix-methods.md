```yaml
number: 1432
title: Split into lint and lint-and-fix methods
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2022-12-29T00:37:36Z
updated_at: 2022-12-29T01:14:34Z
url: https://github.com/astral-sh/ruff/pull/1432
synced_at: 2026-01-12T05:36:31Z
```

# Split into lint and lint-and-fix methods

---

_Pull request opened by @charliermarsh on 2022-12-29 00:37_

This is more verbose, but it lets us perform a small optimization in the non-fix case (we can avoid some string allocations of the entire source code).

---

_@charliermarsh reviewed on 2022-12-29 00:49_

---

_Review comment by @charliermarsh on `src/cache.rs`:137 on 2022-12-29 00:49_

Ah, actually, need to revert this part. We _do_ have to include the autofix setting here. Otherwise, if we run with autofix disabled, then enabled, we'll never pick up the fixes.

---

_Merged by @charliermarsh on 2022-12-29 01:14_

---

_Closed by @charliermarsh on 2022-12-29 01:14_

---

_Branch deleted on 2022-12-29 01:14_

---
