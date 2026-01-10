```yaml
number: 8083
title: Treat resolver failures as fatal in lockfile validation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dynamic-extras
created_at: 2024-10-10T11:32:53Z
updated_at: 2024-10-10T14:01:21Z
url: https://github.com/astral-sh/uv/pull/8083
synced_at: 2026-01-10T12:54:02Z
```

# Treat resolver failures as fatal in lockfile validation

---

_Pull request opened by @charliermarsh on 2024-10-10 11:32_

## Summary

In the routine we use to verify whether the lockfile is up-to-date, we sometimes have to resolve package metadata. If that resolution step fails, the resolver is left in a bad state, as various tasks are marked as pending despite the error. Treating that as a recoverable failure thus leads to a deadlock.

This PR modifies the errors to be treated as fatal.

I think a more holistic fix here would be to add some kind of guard to ensure that any tasks that fail are no longer marked as pending (or enforce this in the type system).

Closes https://github.com/astral-sh/uv/issues/8074.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-10-10 11:33_

---

_Review requested from @konstin by @charliermarsh on 2024-10-10 11:33_

---

_Label `bug` added by @charliermarsh on 2024-10-10 11:33_

---

_Comment by @charliermarsh on 2024-10-10 12:22_

I'm not thrilled with this but it does seem like a reasonable workaround for now.

---

_@BurntSushi approved on 2024-10-10 13:54_

---

_Merged by @charliermarsh on 2024-10-10 14:01_

---

_Closed by @charliermarsh on 2024-10-10 14:01_

---

_Branch deleted on 2024-10-10 14:01_

---
