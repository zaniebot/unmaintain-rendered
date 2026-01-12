```yaml
number: 11597
title: "uv/tests: update snapshots for bad merge"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-bad-merge-conflicting-extras
created_at: 2025-02-18T13:30:36Z
updated_at: 2025-02-18T13:48:13Z
url: https://github.com/astral-sh/uv/pull/11597
synced_at: 2026-01-12T16:09:54Z
```

# uv/tests: update snapshots for bad merge

---

_@BurntSushi_

The bad merge was a result of merging #11293 and #11513. I think even if
I had only merged the former, it still would have resulted in a bad
merge, since the snapshots hadn't been updated for `provide-extras`
additions.

This fixes the build failures seen here:
https://github.com/astral-sh/uv/actions/runs/13390849790/job/37398029632


---

_Review requested from @charliermarsh by @BurntSushi on 2025-02-18 13:30_

---

_Merged by @BurntSushi on 2025-02-18 13:48_

---

_Closed by @BurntSushi on 2025-02-18 13:48_

---

_Branch deleted on 2025-02-18 13:48_

---
