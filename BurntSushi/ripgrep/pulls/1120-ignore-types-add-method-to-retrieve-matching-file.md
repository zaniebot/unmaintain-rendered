```yaml
number: 1120
title: "ignore/types: Add method to retrieve matching file type def from a Glob"
type: pull_request
state: closed
author: firesock
labels: []
assignees: []
base: master
head: add-file-type-def-to-types-glob
created_at: 2018-11-25T21:24:09Z
updated_at: 2019-01-28T00:26:15Z
url: https://github.com/BurntSushi/ripgrep/pull/1120
synced_at: 2026-01-12T18:23:13Z
```

# ignore/types: Add method to retrieve matching file type def from a Glob

---

_@firesock_

Implements first solution from #1116 

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:350 on 2019-01-23 02:41_

Could you fill out the docs a bit more here? Please also use a style consistent with the comments in the surrounding code (e.g., use punctuation).

I don't think you need much more here. But instead of saying, "if any," maybe we can say when a file type def is actually returned, e.g., if there was a match. When there's no match, then there's no available file type def.

---

_@BurntSushi requested changes on 2019-01-23 02:41_

Thanks! This LGTM, just have a small nit on the docs.

---

_Closed by @BurntSushi on 2019-01-24 01:09_

---

_Comment by @BurntSushi on 2019-01-24 01:09_

I ended up just touching up the docs myself and committing the change here https://github.com/BurntSushi/ripgrep/commit/44a9e377371006f01f2e425fbf8d49e43bc14a9b. Thanks!

---

_Comment by @firesock on 2019-01-28 00:26_

Thanks! Sorry, didn't have time to get to it this week

---
