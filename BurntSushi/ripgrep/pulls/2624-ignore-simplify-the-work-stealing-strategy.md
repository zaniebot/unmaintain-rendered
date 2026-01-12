```yaml
number: 2624
title: "ignore: Simplify the work-stealing strategy"
type: pull_request
state: closed
author: tavianator
labels:
  - rollup
assignees: []
base: master
head: simplify-work-stealing
created_at: 2023-10-11T20:24:34Z
updated_at: 2023-11-21T16:29:27Z
url: https://github.com/BurntSushi/ripgrep/pull/2624
synced_at: 2026-01-12T18:23:14Z
```

# ignore: Simplify the work-stealing strategy

---

_@tavianator_

_No description provided._

---

_Comment by @BurntSushi on 2023-10-12 12:21_

Thanks! Out of curiosity, what prompted you to make this change?

---

_Comment by @tavianator on 2023-10-12 12:35_

No particular reason, I happened to be looking at the code again and realized that stealing from your left neighbour or your right neighbour shouldn't make a difference (and indeed perf is the same in my benchmarks)

---

_@BurntSushi approved on 2023-10-12 13:15_

---

_Label `rollup` added by @BurntSushi on 2023-10-12 13:15_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---

_Branch deleted on 2023-11-21 16:29_

---
