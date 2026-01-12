```yaml
number: 955
title: Split install plan into builder and struct
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/plan
created_at: 2024-01-17T20:26:35Z
updated_at: 2024-01-17T20:28:48Z
url: https://github.com/astral-sh/uv/pull/955
synced_at: 2026-01-12T16:04:19Z
```

# Split install plan into builder and struct

---

_@charliermarsh_

The `InstallPlan` does a lot of work in the constructor, which I tend to feel is an anti-pattern. With cache refresh, it's also going to need to be made `async`, so it really feels like it should be a clearer method rather than an async, fallible constructor that does a bunch of IO. This PR splits into a `Planner` (with a `build` method) and a `Plan`.

---

_Label `internal` added by @charliermarsh on 2024-01-17 20:26_

---

_Merged by @charliermarsh on 2024-01-17 20:28_

---

_Closed by @charliermarsh on 2024-01-17 20:28_

---

_Branch deleted on 2024-01-17 20:28_

---
