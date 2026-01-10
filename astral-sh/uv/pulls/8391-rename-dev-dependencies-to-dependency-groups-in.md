```yaml
number: 8391
title: Rename dev dependencies to dependency groups in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: tracking/735
head: charlie/lock-dep-groups
created_at: 2024-10-20T20:13:53Z
updated_at: 2024-10-22T02:56:45Z
url: https://github.com/astral-sh/uv/pull/8391
synced_at: 2026-01-10T12:54:08Z
```

# Rename dev dependencies to dependency groups in lockfile

---

_Pull request opened by @charliermarsh on 2024-10-20 20:13_

## Summary

This is backwards compatible (we respect `dev-dependencies` as an alias).

Part of https://github.com/astral-sh/uv/pull/8272.


---

_Label `enhancement` added by @charliermarsh on 2024-10-20 20:14_

---

_Comment by @T-256 on 2024-10-20 23:54_

> This is backwards compatible

How is this about forward compatibility? i.e. user who has old uv wants to interact with new lockfile from random project?
Should bump the lockfile's version?

---

_Review comment by @j178 on `crates/uv-resolver/src/lock/mod.rs`:2221 on 2024-10-21 02:46_

Should be `dev-dependencies`?

---

_@j178 reviewed on 2024-10-21 02:46_

---

_Comment by @charliermarsh on 2024-10-22 01:52_

> How is this about forward compatibility? i.e. user who has old uv wants to interact with new lockfile from random project?
Should bump the lockfile's version?

Yeah this wouldn't work. I don't think bumping the version even helps here, because prior versions of uv don't look at it and don't (e.g.) throw an error or show a warning.

---

_Review requested from @zanieb by @charliermarsh on 2024-10-22 01:52_

---

_@zanieb approved on 2024-10-22 02:37_

---

_Merged by @charliermarsh on 2024-10-22 02:56_

---

_Closed by @charliermarsh on 2024-10-22 02:56_

---

_Branch deleted on 2024-10-22 02:56_

---
