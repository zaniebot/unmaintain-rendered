```yaml
number: 160
title: Add support for lowest and lowest-direct resolution modes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/resolve
created_at: 2023-10-22T01:11:04Z
updated_at: 2023-10-23T07:31:20Z
url: https://github.com/astral-sh/uv/pull/160
synced_at: 2026-01-10T15:50:28Z
```

# Add support for lowest and lowest-direct resolution modes

---

_Pull request opened by @charliermarsh on 2023-10-22 01:11_

Borrows terminology from pnpm by introducing three resolution modes:

- "Highest": always choose the highest compliant version (default).
- "Lowest": always choose the lowest compliant version.
- "LowestDirect": choose the lowest compliant version of direct dependencies, and the highest compliant version of any transitive dependencies. (This makes a bit more sense than "lowest".)

Closes https://github.com/astral-sh/puffin/issues/142.

---

_Merged by @charliermarsh on 2023-10-22 02:58_

---

_Closed by @charliermarsh on 2023-10-22 02:58_

---

_Branch deleted on 2023-10-22 02:58_

---

_Comment by @konstin on 2023-10-23 07:31_

btw i think we could use this for running integration tests without mocking, the lowest versions shouldn't change through new releases

---
