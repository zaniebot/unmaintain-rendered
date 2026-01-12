```yaml
number: 8509
title: Enforce lockfile schema versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - error messages
assignees: []
merged: true
base: main
head: charlie/lock-version
created_at: 2024-10-23T20:06:15Z
updated_at: 2024-10-28T13:44:10Z
url: https://github.com/astral-sh/uv/pull/8509
synced_at: 2026-01-12T16:08:21Z
```

# Enforce lockfile schema versions

---

_@charliermarsh_

## Summary

Historically, we haven't enforced schema versions. This PR adds a versioning policy such that, if a uv version writes schema v2, then...

- It will always reject lockfiles with schema v3 or later.
- It _may_ reject lockfiles with schema v1, but can also choose to read them, if possible.

(For example, the change we proposed to rename `dev-dependencies` to `dependency-groups` would've been backwards-compatible: newer versions of uv could still read lockfiles that used the `dev-dependencies` field name, but older versions should reject lockfiles that use the `dependency-groups` field name.)

Closes https://github.com/astral-sh/uv/issues/8465.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-10-23 20:22_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-23 20:22_

---

_Label `enhancement` added by @charliermarsh on 2024-10-23 20:22_

---

_Label `error messages` added by @charliermarsh on 2024-10-23 20:22_

---

_@zanieb approved on 2024-10-24 16:19_

---

_Merged by @charliermarsh on 2024-10-24 16:23_

---

_Closed by @charliermarsh on 2024-10-24 16:23_

---

_Branch deleted on 2024-10-24 16:23_

---

_Comment by @BurntSushi on 2024-10-28 13:44_

Makes sense to me!

---
