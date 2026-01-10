```yaml
number: 2019
title: "pep440: remove redundant `without_local()`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/pep440-version-spec-contains
created_at: 2024-02-27T15:46:40Z
updated_at: 2024-02-27T16:00:59Z
url: https://github.com/astral-sh/uv/pull/2019
synced_at: 2026-01-10T14:54:43Z
```

# pep440: remove redundant `without_local()`

---

_Pull request opened by @BurntSushi on 2024-02-27 15:46_

In this context, we already know (as the comment says) that `self` does
not have a local segment, so we don't need to strip it.

This change isn't motivated by anything other than making the code and
comment in sync. For example, when I first looked at it, I wondered
whether the extra stripping was somehow necessary. But it isn't.


---

_Review requested from @konstin by @BurntSushi on 2024-02-27 15:46_

---

_@konstin approved on 2024-02-27 15:51_

---

_Merged by @BurntSushi on 2024-02-27 16:00_

---

_Closed by @BurntSushi on 2024-02-27 16:00_

---

_Branch deleted on 2024-02-27 16:00_

---
