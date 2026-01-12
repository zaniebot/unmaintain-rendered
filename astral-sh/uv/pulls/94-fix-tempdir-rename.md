```yaml
number: 94
title: Fix tempdir rename
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix-tempdir-rename
created_at: 2023-10-12T18:40:03Z
updated_at: 2023-10-12T18:47:40Z
url: https://github.com/astral-sh/uv/pull/94
synced_at: 2026-01-12T16:03:45Z
```

# Fix tempdir rename

---

_@konstin_

This fixes two bugs on linux:

`/tmp` and `$HOME` are technically on two different partitions on my machine, which means that rename-as-atomic-dir-write doesn't work. The solution is to create the temp dir in the target directory.

zip files may contain directory entries, we can't create files for them but need to create directories. We could skip them though because iirc they are not in the RECORD so they won't be uninstalled.

---

_Merged by @konstin on 2023-10-12 18:47_

---

_Closed by @konstin on 2023-10-12 18:47_

---

_Branch deleted on 2023-10-12 18:47_

---
