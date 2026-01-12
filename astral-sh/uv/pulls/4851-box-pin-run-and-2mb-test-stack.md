```yaml
number: 4851
title: "`Box::pin(run())` and 2MB test stack"
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/box-pin-run-and-decrease-test-stack-size
created_at: 2024-07-06T21:03:04Z
updated_at: 2024-07-08T00:50:47Z
url: https://github.com/astral-sh/uv/pull/4851
synced_at: 2026-01-12T16:06:29Z
```

# `Box::pin(run())` and 2MB test stack

---

_@konstin_

By using `Box::pin(run())` we can reduce the artificial stack size for running tests on windows in debug mode from 8MB to 2MB. I've checked and 1MB/no custom stack size still fail tests, e.g. `add_workspace_editable`.

---

_Label `internal` added by @konstin on 2024-07-06 21:03_

---

_Label `windows` added by @konstin on 2024-07-06 21:03_

---

_Review requested from @ibraheemdev by @konstin on 2024-07-06 21:03_

---

_@ibraheemdev approved on 2024-07-06 21:50_

Nice!

Maybe we should add a comment explaining why this is necessary?

---

_Merged by @charliermarsh on 2024-07-08 00:50_

---

_Closed by @charliermarsh on 2024-07-08 00:50_

---

_Branch deleted on 2024-07-08 00:50_

---

_Comment by @charliermarsh on 2024-07-08 00:50_

I need this as some of my PRs are now failing due to stack size. Hope that's cool.

---
