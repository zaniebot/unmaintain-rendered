```yaml
number: 9103
title: "Rename `ResolutionGraph` to `ResolverOutput`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/out
created_at: 2024-11-13T23:23:50Z
updated_at: 2024-11-14T14:51:13Z
url: https://github.com/astral-sh/uv/pull/9103
synced_at: 2026-01-10T12:00:00Z
```

# Rename `ResolutionGraph` to `ResolverOutput`

---

_Pull request opened by @charliermarsh on 2024-11-13 23:23_

## Summary

As discussed in Discord... This struct has evolved to include a lot of information apart from the `petgraph::Graph`. And I want to add a graph to the simplified `Resolution` type. So I think this name makes more sense.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-13 23:23_

---

_Review requested from @konstin by @charliermarsh on 2024-11-13 23:23_

---

_Label `internal` added by @charliermarsh on 2024-11-13 23:23_

---

_Review request for @zanieb removed by @charliermarsh on 2024-11-13 23:24_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-13 23:24_

---

_Marked ready for review by @charliermarsh on 2024-11-13 23:24_

---

_@konstin approved on 2024-11-14 09:09_

---

_@BurntSushi approved on 2024-11-14 13:36_

---

_Merged by @charliermarsh on 2024-11-14 14:51_

---

_Closed by @charliermarsh on 2024-11-14 14:51_

---

_Branch deleted on 2024-11-14 14:51_

---
