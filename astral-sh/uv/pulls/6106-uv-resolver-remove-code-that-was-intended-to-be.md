```yaml
number: 6106
title: "uv-resolver: remove code that was intended to be removed"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/allow-forking-with-prefs
created_at: 2024-08-15T11:36:54Z
updated_at: 2024-08-15T12:25:57Z
url: https://github.com/astral-sh/uv/pull/6106
synced_at: 2026-01-10T13:09:50Z
```

# uv-resolver: remove code that was intended to be removed

---

_Pull request opened by @BurntSushi on 2024-08-15 11:36_

In particular, I added this as a hack to avoid a kinda of
instability that was caused by our marker code not correctly
detecting markers that were always false. But that has since
been fixed.

Removing this code doesn't change any tests. Arguably it
should be possible to come up with a test that failed with
this hack inserted but succeeded without it. In particular,
with this hack, new forks were being prevented from being
added even when they ought to be added, e.g., when preferences
get updated.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-15 11:37_

---

_@charliermarsh approved on 2024-08-15 11:55_

---

_Merged by @BurntSushi on 2024-08-15 12:25_

---

_Closed by @BurntSushi on 2024-08-15 12:25_

---

_Branch deleted on 2024-08-15 12:25_

---
