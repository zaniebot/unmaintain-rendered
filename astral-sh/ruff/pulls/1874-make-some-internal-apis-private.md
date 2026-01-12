```yaml
number: 1874
title: Make some internal APIs private
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: privatize-internals
created_at: 2023-01-14T19:37:07Z
updated_at: 2023-01-14T23:23:59Z
url: https://github.com/astral-sh/ruff/pull/1874
synced_at: 2026-01-12T05:36:32Z
```

# Make some internal APIs private

---

_Pull request opened by @not-my-profile on 2023-01-14 19:37_

_No description provided._

---

_Renamed from "Make internal APIs private" to "Make some internal APIs private" by @not-my-profile on 2023-01-14 19:37_

---

_@charliermarsh reviewed on 2023-01-14 23:23_

---

_Review comment by @charliermarsh on `ruff_cli/src/cli.rs`:387 on 2023-01-14 23:23_

We should probably just remove this. It's sort of incongruous with the other CLI options we provide. But can be a separate change.

---

_Merged by @charliermarsh on 2023-01-14 23:23_

---

_Closed by @charliermarsh on 2023-01-14 23:23_

---
