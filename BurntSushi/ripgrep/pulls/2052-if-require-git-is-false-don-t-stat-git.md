```yaml
number: 2052
title: "If require_git is false, don't stat .git"
type: pull_request
state: merged
author: joshtriplett
labels: []
assignees: []
merged: true
base: master
head: dont-stat-git-if-not-requiring-git
created_at: 2021-11-07T23:14:19Z
updated_at: 2021-11-12T13:37:05Z
url: https://github.com/BurntSushi/ripgrep/pull/2052
synced_at: 2026-01-12T18:23:14Z
```

# If require_git is false, don't stat .git

---

_@joshtriplett_

I've confirmed via strace that this eliminates a pile of stat calls.

---

_@BurntSushi approved on 2021-11-09 15:40_

LGTM! I think this just has to be rustfmt'd. :)

---

_Comment by @joshtriplett on 2021-11-11 12:50_

rustfmt'd. :)

---

_Merged by @BurntSushi on 2021-11-12 13:37_

---

_Closed by @BurntSushi on 2021-11-12 13:37_

---
