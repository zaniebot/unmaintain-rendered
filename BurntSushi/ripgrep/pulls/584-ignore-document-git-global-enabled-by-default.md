```yaml
number: 584
title: "ignore: document git_global enabled by default"
type: pull_request
state: merged
author: durka
labels: []
assignees: []
merged: true
base: master
head: patch-2
created_at: 2017-08-25T18:01:02Z
updated_at: 2017-08-26T18:49:41Z
url: https://github.com/BurntSushi/ripgrep/pull/584
synced_at: 2026-01-12T18:23:13Z
```

# ignore: document git_global enabled by default

---

_@durka_

This function doesn't say it's enabled by default, but it looks like it is, based on my reading of `dir.rs`: https://github.com/BurntSushi/ripgrep/blob/30608f2444a2abc7aae8e7b96cd5cff4f98bfe71/ignore/src/dir.rs#L429-L435

---

_Comment by @BurntSushi on 2017-08-26 18:49_

Yup, that's right! Thanks!

---

_Merged by @BurntSushi on 2017-08-26 18:49_

---

_Closed by @BurntSushi on 2017-08-26 18:49_

---
