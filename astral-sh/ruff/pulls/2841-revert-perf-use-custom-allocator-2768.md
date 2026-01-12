```yaml
number: 2841
title: "Revert \"perf: Use custom allocator (#2768)\""
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2023-02-13T03:28:38Z
updated_at: 2023-02-13T03:31:36Z
url: https://github.com/astral-sh/ruff/pull/2841
synced_at: 2026-01-12T04:52:01Z
```

# Revert "perf: Use custom allocator (#2768)"

---

_Pull request opened by @charliermarsh on 2023-02-13 03:28_

This is causing wheel creation to fail on some of our more exotic build targets: https://github.com/charliermarsh/ruff/actions/runs/4159524132.

Let's figure out how to gate appropriately, but for now, reverting to get the release out.

---

_Merged by @charliermarsh on 2023-02-13 03:31_

---

_Closed by @charliermarsh on 2023-02-13 03:31_

---

_Branch deleted on 2023-02-13 03:31_

---
