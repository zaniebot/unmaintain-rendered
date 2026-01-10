```yaml
number: 9582
title: "re-enable conflicting extra/group tests and fix regression from #9540"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/enable-lock-conflict-tests
created_at: 2024-12-02T19:51:00Z
updated_at: 2024-12-02T20:39:40Z
url: https://github.com/astral-sh/uv/pull/9582
synced_at: 2026-01-10T12:00:00Z
```

# re-enable conflicting extra/group tests and fix regression from #9540

---

_Pull request opened by @BurntSushi on 2024-12-02 19:51_

It turns out that #9540 _did_ regress some existing tests, but it went
unnoticed because of a gaffe by myself: #9474 moved the conflicting
extra/group tests, but never enabled them!

This PR re-enables them, updates the snapshots and fixes the regression
from #9540. The commit messages give more detail.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-12-02 19:51_

---

_@charliermarsh approved on 2024-12-02 19:53_

---

_Merged by @BurntSushi on 2024-12-02 20:39_

---

_Closed by @BurntSushi on 2024-12-02 20:39_

---

_Branch deleted on 2024-12-02 20:39_

---
