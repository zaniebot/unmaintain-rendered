```yaml
number: 767
title: "search: add support for searching compressed files"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/compressed
created_at: 2018-01-30T02:26:46Z
updated_at: 2018-01-30T14:14:06Z
url: https://github.com/BurntSushi/ripgrep/pull/767
synced_at: 2026-01-12T18:23:13Z
```

# search: add support for searching compressed files

---

_@BurntSushi_

This commit adds opt-in support for searching compressed files during
recursive search. This behavior is only enabled when the
`-z/--search-zip` flag is passed to ripgrep. When enabled, a limited set
of common compression formats are recognized via file extension, and a
new process is spawned to perform the decompression. ripgrep then
searches the stdout of that spawned process.

Closes #539 

---

This PR is based on work in #751. Most of that PR was kept unchanged, with one exception: we need to more carefully manage the subprocess. I will leave comments on #751 explaining it.

---

_Merged by @BurntSushi on 2018-01-30 14:13_

---

_Closed by @BurntSushi on 2018-01-30 14:13_

---

_Branch deleted on 2018-01-30 14:14_

---
