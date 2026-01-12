```yaml
number: 14876
title: Avoid invalidating lockfile when path or workspace dependencies define explicit indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/explicit
created_at: 2025-07-24T19:47:42Z
updated_at: 2025-07-25T12:18:30Z
url: https://github.com/astral-sh/uv/pull/14876
synced_at: 2026-01-12T16:11:27Z
```

# Avoid invalidating lockfile when path or workspace dependencies define explicit indexes

---

_@charliermarsh_

## Summary

This is an alternative to #14003 that takes advantage of the fact that we already validate that the requirements are up-to-date when validating the lockfile, and the requirements for pinned requirements include the index itself -- so rather than collecting all the explicit indexes upfront, we can just add them to the available list as we iterate over the lockfile's dependency graph.

This gets all the tests passing from that PR, but with ~no performance impact and a much less invasive change. It also gets the "circular dependency" test passing, which is marked with a TODO in that PR.

Closes https://github.com/astral-sh/uv/issues/11419.


---

_Label `bug` added by @charliermarsh on 2025-07-24 19:47_

---

_Marked ready for review by @charliermarsh on 2025-07-24 19:47_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-24 20:28_

---

_Review requested from @jtfmumm by @charliermarsh on 2025-07-24 20:28_

---

_Review requested from @konstin by @charliermarsh on 2025-07-24 20:28_

---

_@jtfmumm approved on 2025-07-25 09:40_

---

_Merged by @charliermarsh on 2025-07-25 12:18_

---

_Closed by @charliermarsh on 2025-07-25 12:18_

---

_Branch deleted on 2025-07-25 12:18_

---
