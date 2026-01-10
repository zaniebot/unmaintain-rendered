```yaml
number: 1203
title: Report number of bytes deleted when clearing cache
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/tally
created_at: 2024-01-31T06:30:12Z
updated_at: 2024-01-31T15:48:29Z
url: https://github.com/astral-sh/uv/pull/1203
synced_at: 2026-01-10T15:39:03Z
```

# Report number of bytes deleted when clearing cache

---

_Pull request opened by @charliermarsh on 2024-01-31 06:30_

## Summary

This is based on Cargo's `clean` implementation, with modifications based on some of my own preferences, and to better adhere to patterns we use in our codebase:

![Screenshot 2024-01-31 at 1 31 10 AM](https://github.com/astral-sh/puffin/assets/1309177/38704798-b17f-4972-ab67-00484ce63d62)


---

_Renamed from "gsReport number of bytes deleted when clearing cache" to "Report number of bytes deleted when clearing cache" by @charliermarsh on 2024-01-31 06:30_

---

_Marked ready for review by @charliermarsh on 2024-01-31 06:32_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-31 06:32_

---

_Review comment by @konstin on `crates/puffin/src/commands/clean.rs`:136 on 2024-01-31 12:32_

Can we use `humansize` here? I remember we used to have that in the download code, but it only depends on libm anyway

---

_@konstin approved on 2024-01-31 12:33_

---

_Review comment by @BurntSushi on `crates/puffin-cache/src/lib.rs`:605 on 2024-01-31 14:06_

This was nicely done. Very clean.

---

_Review comment by @BurntSushi on `crates/puffin/src/commands/clean.rs`:136 on 2024-01-31 14:09_

I'll vote in favor of skipping the dependency. :) But I don't feel strongly.

---

_@BurntSushi approved on 2024-01-31 14:10_

Nice!

---

_@zanieb reviewed on 2024-01-31 14:48_

---

_Review comment by @zanieb on `crates/puffin/tests/pip_sync.rs`:1617 on 2024-01-31 14:48_

Seems dangerous to have the sizes in the snapshots — are these constant?

---

_@charliermarsh reviewed on 2024-01-31 15:28_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_sync.rs`:1617 on 2024-01-31 15:28_

Hah, apparently not, hence test failures. Will fix.

---

_Merged by @charliermarsh on 2024-01-31 15:48_

---

_Closed by @charliermarsh on 2024-01-31 15:48_

---

_Branch deleted on 2024-01-31 15:48_

---
