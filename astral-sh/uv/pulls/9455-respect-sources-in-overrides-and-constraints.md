```yaml
number: 9455
title: Respect sources in overrides and constraints
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/sources
created_at: 2024-11-26T22:40:44Z
updated_at: 2024-11-27T13:56:16Z
url: https://github.com/astral-sh/uv/pull/9455
synced_at: 2026-01-10T12:00:00Z
```

# Respect sources in overrides and constraints

---

_Pull request opened by @charliermarsh on 2024-11-26 22:40_

## Summary

We still only respect overrides and constraints in the workspace root -- which we may want to change -- but overrides and constraints are now correctly lowered.

Closes https://github.com/astral-sh/uv/issues/8148.


---

_Label `enhancement` added by @charliermarsh on 2024-11-26 22:40_

---

_Marked ready for review by @charliermarsh on 2024-11-26 23:00_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-26 23:00_

---

_Review requested from @konstin by @charliermarsh on 2024-11-26 23:00_

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:1436 on 2024-11-26 23:09_

```suggestion
/// Lock a project with `uv.tool.override-dependencies` that reference `tool.uv.sources`.
```

---

_@konstin approved on 2024-11-26 23:13_

---

_@zanieb approved on 2024-11-27 01:48_

Perhaps worth noting in the dependencies documentation page too.

---

_Merged by @charliermarsh on 2024-11-27 13:56_

---

_Closed by @charliermarsh on 2024-11-27 13:56_

---

_Branch deleted on 2024-11-27 13:56_

---
