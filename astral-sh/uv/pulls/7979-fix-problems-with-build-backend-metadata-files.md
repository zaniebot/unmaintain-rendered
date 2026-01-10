```yaml
number: 7979
title: " Fix problems with build backend metadata files"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend7-fixes-to-hashes-and-entrypoints
created_at: 2024-10-07T17:34:17Z
updated_at: 2024-10-08T14:21:40Z
url: https://github.com/astral-sh/uv/pull/7979
synced_at: 2026-01-10T12:54:00Z
```

#  Fix problems with build backend metadata files

---

_Pull request opened by @konstin on 2024-10-07 17:34_

Additional work on top of https://github.com/astral-sh/uv/pull/7966:

* Snapshot all the metadata files we write
* Only write `entry_points.txt` when there are any entrypoints
* Fix hashing with the filesystem writer
* Some code style improvements

---

_Review requested from @BurntSushi by @konstin on 2024-10-07 17:34_

---

_@BurntSushi approved on 2024-10-08 14:18_

---

_Label `preview` added by @konstin on 2024-10-08 14:21_

---

_Merged by @konstin on 2024-10-08 14:21_

---

_Closed by @konstin on 2024-10-08 14:21_

---

_Branch deleted on 2024-10-08 14:21_

---
