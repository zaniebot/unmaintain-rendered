```yaml
number: 365
title: Use a custom PubGrub error type to always show resolution report
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/error-type
created_at: 2023-11-08T05:03:14Z
updated_at: 2023-11-08T14:57:27Z
url: https://github.com/astral-sh/uv/pull/365
synced_at: 2026-01-10T15:50:28Z
```

# Use a custom PubGrub error type to always show resolution report

---

_Pull request opened by @charliermarsh on 2023-11-08 05:03_

Closes https://github.com/astral-sh/puffin/issues/356.

The example from the issue now renders as:

```
❯ cargo run --bin puffin-dev -q -- resolve-cli tensorflow-cpu-aws
puffin-dev failed
  Caused by: No solution found when resolving build dependencies for source distribution:
  Caused by: Because there is no available version for tensorflow-cpu-aws and root depends on tensorflow-cpu-aws, version solving failed.
```

---

_Review requested from @zanieb by @charliermarsh on 2023-11-08 05:03_

---

_Review requested from @konstin by @charliermarsh on 2023-11-08 05:03_

---

_Comment by @charliermarsh on 2023-11-08 05:03_

Relatedly, I am still interested in using Miette for all the user-facing reporting.

---

_@konstin approved on 2023-11-08 10:06_

---

_Merged by @charliermarsh on 2023-11-08 14:57_

---

_Closed by @charliermarsh on 2023-11-08 14:57_

---

_Branch deleted on 2023-11-08 14:57_

---
