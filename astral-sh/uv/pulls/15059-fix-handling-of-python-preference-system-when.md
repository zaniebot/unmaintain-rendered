```yaml
number: 15059
title: "Fix handling of `python-preference = system` when managed interpreters are on the PATH"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/system-fix
created_at: 2025-08-04T14:12:10Z
updated_at: 2025-08-04T16:02:43Z
url: https://github.com/astral-sh/uv/pull/15059
synced_at: 2026-01-12T16:11:33Z
```

# Fix handling of `python-preference = system` when managed interpreters are on the PATH

---

_@zanieb_

This is the first part of fixing a 0.8.0 regression from https://github.com/astral-sh/uv/pull/7934

There, we added handling for skipping managed interpreters on the PATH when `only-system` is used, but did not update the logic to prefer system interpreters over managed ones when `system` is used. Here, we fix that by skipping managed interpreters when `system` is used unless _only_ managed interpreters are available. While this logic is applied during in a general discovery method, it's only relevant for the PATH (and the Windows registry) because we already change the _order_ that we inspect installations in when `system` is used, so the managed installation directory is inspected last.

This behavior did not regress in 0.8, it's always been this way, however, I need this change in order to fix a different bug.

---

_Label `bug` added by @zanieb on 2025-08-04 14:12_

---

_Renamed from "zb/system fix" to "Fix handling of `python-preference = system` when managed interpreters are on the PATH" by @zanieb on 2025-08-04 14:27_

---

_@zanieb reviewed on 2025-08-04 14:39_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:789 on 2025-08-04 14:39_

This test case failed prior to this pull request, it'd discover 3.11.

---

_@zanieb reviewed on 2025-08-04 14:39_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:942 on 2025-08-04 14:39_

Moved without change

---

_Marked ready for review by @zanieb on 2025-08-04 14:40_

---

_@jtfmumm reviewed on 2025-08-04 15:44_

---

_Review comment by @jtfmumm on `crates/uv-python/src/discovery.rs`:1347 on 2025-08-04 15:44_

Should we log something here since we log skipping this installation earlier?

---

_@jtfmumm approved on 2025-08-04 15:45_

---

_@zanieb reviewed on 2025-08-04 15:47_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1347 on 2025-08-04 15:47_

We don't for `first_prerelease`, so I just copied that pattern. It does seem reasonable though.

---

_@zanieb reviewed on 2025-08-04 15:50_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1347 on 2025-08-04 15:50_

Added

---

_Merged by @zanieb on 2025-08-04 16:02_

---

_Closed by @zanieb on 2025-08-04 16:02_

---

_Branch deleted on 2025-08-04 16:02_

---
