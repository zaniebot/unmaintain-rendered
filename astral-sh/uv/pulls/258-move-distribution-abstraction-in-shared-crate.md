```yaml
number: 258
title: Move distribution abstraction in shared crate
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pinned-package
created_at: 2023-10-31T19:27:26Z
updated_at: 2023-10-31T19:30:07Z
url: https://github.com/astral-sh/uv/pull/258
synced_at: 2026-01-12T16:03:50Z
```

# Move distribution abstraction in shared crate

---

_@charliermarsh_

This also allows us to get rid of `PinnedPackage` _and_ to remove some `Result<...>` types due to needless conversions between otherwise-identical types.

---

_Marked ready for review by @charliermarsh on 2023-10-31 19:27_

---

_Merged by @charliermarsh on 2023-10-31 19:30_

---

_Closed by @charliermarsh on 2023-10-31 19:30_

---

_Branch deleted on 2023-10-31 19:30_

---
