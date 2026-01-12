```yaml
number: 1757
title: "fix 'pip install' for zips with bunk permissions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fix-1453
created_at: 2024-02-20T15:03:53Z
updated_at: 2024-02-20T15:57:51Z
url: https://github.com/astral-sh/uv/pull/1757
synced_at: 2026-01-12T16:04:43Z
```

# fix 'pip install' for zips with bunk permissions

---

_@BurntSushi_

This fixes an issue where installing a source dist from a `zip` file could
fail if the permissions in the `zip` file were bunk. Namely, after extracting
a `zip` file, we go through each record and set the permissions on each file
based on the permissions recorded in the `zip` file. But apparently, those
permissions are sometimes present but `0`, which in turn caused us to set the
permissions to `0` on files and thus prevent our access to them.

We fix this by treating "absent permissions" and "permissions of mode 0" as the
same thing.

Fixes #1453


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-20 15:04_

---

_Comment by @BurntSushi on 2024-02-20 15:13_

This fix is actually duplicative with https://github.com/astral-sh/uv/pull/1743, which also fixes #1453 (but in a better way). So I removed the "fix" from this PR, but kept the regression test and a couple other small fixups. Once #1743 is merged, we should be able to bring this one in. (CI for this branch will fail on the regression test until then.)

---

_Label `internal` added by @BurntSushi on 2024-02-20 15:38_

---

_Merged by @BurntSushi on 2024-02-20 15:57_

---

_Closed by @BurntSushi on 2024-02-20 15:57_

---

_Branch deleted on 2024-02-20 15:57_

---
