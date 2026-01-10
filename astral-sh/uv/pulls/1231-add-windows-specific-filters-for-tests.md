```yaml
number: 1231
title: Add windows specific filters for tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - windows
assignees: []
merged: true
base: main
head: konsti/windows-filters
created_at: 2024-02-01T17:26:24Z
updated_at: 2024-02-06T14:58:17Z
url: https://github.com/astral-sh/uv/pull/1231
synced_at: 2026-01-10T15:33:24Z
```

# Add windows specific filters for tests

---

_Pull request opened by @konstin on 2024-02-01 17:26_

Add more windows specific filters in various places.

435 tests run: 333 passed (21 slow), 102 failed, 1 skipped

---

_Label `internal` added by @konstin on 2024-02-01 17:26_

---

_Label `windows` added by @konstin on 2024-02-01 17:26_

---

_@konstin reviewed on 2024-02-05 21:26_

---

_Review comment by @konstin on `crates/puffin-cache/src/removal.rs`:56 on 2024-02-05 21:26_

Not entirely sure about this, but it seems to match the unix values closer

---

_Renamed from "Add windows specific filters" to "Add windows specific filters for tests" by @konstin on 2024-02-05 21:51_

---

_Marked ready for review by @konstin on 2024-02-05 22:09_

---

_Review requested from @charliermarsh by @konstin on 2024-02-05 22:09_

---

_@charliermarsh reviewed on 2024-02-06 00:35_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/removal.rs`:57 on 2024-02-06 00:35_

You tested that this works, right? Like, that we can remove a junction this way?

---

_@charliermarsh reviewed on 2024-02-06 00:36_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/removal.rs`:57 on 2024-02-06 00:36_

(What if it's some other kind of symlink?)

---

_@charliermarsh approved on 2024-02-06 00:39_

LGTM assuming the symlink removes work as expected...

---

_@charliermarsh reviewed on 2024-02-06 00:39_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/removal.rs`:57 on 2024-02-06 00:39_

(Like what if it's a "real" symlink?)

---

_@konstin reviewed on 2024-02-06 14:57_

---

_Review comment by @konstin on `crates/puffin-cache/src/removal.rs`:57 on 2024-02-06 14:57_

It seems to work in my manual tests.

The implementation assumes we are using junctions on windows, we have to update the code should we ever switch to symlinks.

---

_Merged by @konstin on 2024-02-06 14:58_

---

_Closed by @konstin on 2024-02-06 14:58_

---

_Branch deleted on 2024-02-06 14:58_

---
