```yaml
number: 892
title: "ignore: impl Clone for DirEntry"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fixes
created_at: 2018-04-21T15:42:04Z
updated_at: 2018-04-21T16:02:51Z
url: https://github.com/BurntSushi/ripgrep/pull/892
synced_at: 2026-01-12T18:23:13Z
```

# ignore: impl Clone for DirEntry

---

_@BurntSushi_

There is a small hiccup here in that a `DirEntry` can embed errors
associated with reading an ignore file, which can be accessed and logged
by consumers if desired. That error type can contain an io::Error, which
isn't cloneable. We therefore implement Clone on our library's error
type in a way that re-creates the I/O error as best as possible.

Fixes #891


---

_Comment by @BurntSushi on 2018-04-21 15:42_

cc @stevedonovan

---

_Merged by @BurntSushi on 2018-04-21 16:01_

---

_Closed by @BurntSushi on 2018-04-21 16:01_

---

_Branch deleted on 2018-04-21 16:02_

---
