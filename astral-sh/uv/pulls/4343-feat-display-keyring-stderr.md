```yaml
number: 4343
title: "feat: display keyring stderr"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: keyring-stderr
created_at: 2024-06-16T18:02:47Z
updated_at: 2024-06-17T23:48:21Z
url: https://github.com/astral-sh/uv/pull/4343
synced_at: 2026-01-10T13:54:02Z
```

# feat: display keyring stderr

---

_Pull request opened by @samypr100 on 2024-06-16 18:02_

## Summary

Closes https://github.com/astral-sh/uv/issues/4162

Changes keyring subprocess to allow display of stderr.
This aligns with pip's behavior since pip 23.1.

## Test Plan

* Tested using gnome-keyring-backend on a self-hosted private registry as well as the keyring script described in #4162 to confirm both existing functionality and the new stderr display.
* Existing tests using `scripts/packages/keyring_test_plugin` are now showing its stderr output as well.

---

_Assigned to @zanieb by @zanieb on 2024-06-16 19:29_

---

_Marked ready for review by @samypr100 on 2024-06-16 19:30_

---

_Review requested from @zanieb by @konstin on 2024-06-17 08:58_

---

_@zanieb approved on 2024-06-17 18:29_

Thanks!

---

_Merged by @zanieb on 2024-06-17 18:29_

---

_Closed by @zanieb on 2024-06-17 18:29_

---

_Label `enhancement` added by @zanieb on 2024-06-17 18:29_

---

_Branch deleted on 2024-06-17 23:48_

---
