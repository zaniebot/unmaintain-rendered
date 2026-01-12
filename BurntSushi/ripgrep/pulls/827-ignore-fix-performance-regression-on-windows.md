```yaml
number: 827
title: "ignore: fix performance regression on Windows"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-perf
created_at: 2018-02-21T00:36:14Z
updated_at: 2018-02-21T00:50:56Z
url: https://github.com/BurntSushi/ripgrep/pull/827
synced_at: 2026-01-12T18:23:13Z
```

# ignore: fix performance regression on Windows

---

_@BurntSushi_

This commit fixes a performance regression in Windows that resulted from
fallout from fixing #705. In particular, we introduced an additional
stat call for every single directory entry, which can be quite
disastrous for performance.

There is a corresponding companion PR that fixes the same bug in
walkdir: https://github.com/BurntSushi/walkdir/pull/96

Fixes #820

---

_Merged by @BurntSushi on 2018-02-21 00:50_

---

_Closed by @BurntSushi on 2018-02-21 00:50_

---

_Branch deleted on 2018-02-21 00:50_

---
