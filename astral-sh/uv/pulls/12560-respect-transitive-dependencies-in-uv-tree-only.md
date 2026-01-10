```yaml
number: 12560
title: "Respect transitive dependencies in `uv tree --only-group`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/only
created_at: 2025-03-30T18:39:02Z
updated_at: 2025-03-30T18:48:49Z
url: https://github.com/astral-sh/uv/pull/12560
synced_at: 2026-01-10T11:10:40Z
```

# Respect transitive dependencies in `uv tree --only-group`

---

_Pull request opened by @charliermarsh on 2025-03-30 18:39_

## Summary

The overall strategy here is to make this code look more like `requirements_txt.rs`: we seed the root members, then perform a DFS. Previously, we created all nodes upfront, which caused problems when using `--only-group`, since we'd omit "production" dependencies of development dependencies.

Closes https://github.com/astral-sh/uv/issues/12526.


---

_Label `bug` added by @charliermarsh on 2025-03-30 18:39_

---

_Marked ready for review by @charliermarsh on 2025-03-30 18:39_

---

_Renamed from "Respect transitive dependencies in uv tree --only-group" to "Respect transitive dependencies in `uv tree --only-group`" by @charliermarsh on 2025-03-30 18:39_

---

_Merged by @charliermarsh on 2025-03-30 18:48_

---

_Closed by @charliermarsh on 2025-03-30 18:48_

---

_Branch deleted on 2025-03-30 18:48_

---
