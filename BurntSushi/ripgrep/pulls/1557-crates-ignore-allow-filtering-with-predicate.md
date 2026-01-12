```yaml
number: 1557
title: "crates/ignore: Allow filtering with predicate"
type: pull_request
state: closed
author: casey
labels:
  - rollup
assignees: []
base: master
head: ignore-filter
created_at: 2020-04-21T01:46:00Z
updated_at: 2020-05-09T03:30:43Z
url: https://github.com/BurntSushi/ripgrep/pull/1557
synced_at: 2026-01-12T18:23:14Z
```

# crates/ignore: Allow filtering with predicate

---

_@casey_

Adds `WalkBuilder::filter_entry` that takes a predicate to be applied to
all entries. If the predicate returns `false` on a given entry, that
entry and all children will be skipped.

Fixes: #1555

---

_@BurntSushi approved on 2020-05-09 02:21_

Outstanding, thank you!

---

_Label `rollup` added by @BurntSushi on 2020-05-09 02:22_

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---

_Comment by @casey on 2020-05-09 03:30_

You're most welcome, thanks for merging!

---

_Branch deleted on 2020-05-09 03:30_

---
